import time

import requests
import json

headers = {
    'Accept': 'application/json',
    'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.0.0 Safari/537.36',
    'Cookie':'换成你自己的cookie'
}

# 过滤函数
def filter_response(response):
    if response.content.startswith(b'<!doctype html>'):
        return None
    return response

# 设置参数
page = 0
limit = 2
keyword = '上海'

result_dict = {}

while True:
    # 设置搜索时的请求连接以及请求头
    if page:
        url = f'https://m.weibo.cn/api/container/getIndex?containerid=100103type%3D1%26q%3D{keyword}&page_type=searchall&page={page}'
    else:
        url = f'https://m.weibo.cn/api/container/getIndex?containerid=100103type%3D1%26q%3D{keyword}&page_type=searchall'

    # 开始爬取内容
    response = requests.get(url=url, headers=headers)

    # 第一步在data.cardlistInfo中提取出total以及当前页码
    # 第二捕，设置index遍历data.cards，先检查cards中的card_type中的值，是否为9，如果为9，则开始爬取需要的内容
    # 爬取cards.mblog下的id、mid、title、text、reposts_count(转发数）、attitudes_count（点赞数）、comments_count（评论数）、created_at（发布时间）、 status_province（省份）、user下的id、screen_name（昵称）

    # 将url的返回值解析为json格式利于计算
    res = response.json()
    resData = res['data']  # 提取res中的data数据
    # 提起cardlistInfo中的total以及当前页码，然后用total除page_size得到所有的page进行提取

    # todo 根据total和page 爬取所有搜索到的页面下的帖子并保存
    # json_dict_2 = json.dumps(resData, indent=2, sort_keys=True, ensure_ascii=False)
    # print(json_dict_2)
    # print(url)

    page = resData['cardlistInfo']['page']


    # 检查cards下的card_type，然后提取
    # 先提出card中的值
    cards = resData['cards']  # 列表元素

    # todo 将每个帖子下的面的评论内容，以及评论用户和评论时间，ip保存在字典中comment_details下
    dict_ = {}
    for card in cards:
        ## print(card)
        if card['card_type'] == 11:
            card_group = card['card_group']
            for cgList in card_group:
                # print(cgList)
                if cgList['card_type'] == 9:
                    # print('mblog:',cgList['mblog']['id'])
                    temp_dict = {
                        # 'id':cgList['mblog']['id'],
                        # 以帖子id为key，然后记录每个帖子下的相关信息
                        cgList['mblog']['id']: {
                            'mid': cgList['mblog']['mid'],
                            'text': cgList['mblog']['text'],
                            'reposts_count': cgList['mblog']['reposts_count'],
                            'attitudes_count': cgList['mblog']['attitudes_count'],
                            'comments_count': cgList['mblog']['comments_count'],
                            'comments_details': {},
                            'created_at': cgList['mblog']['created_at'],
                            'status_province': cgList['mblog']['status_province'],
                            'user_id': cgList['mblog']['user']['id'],
                            'screen_name': cgList['mblog']['user']['screen_name']
                        }

                    }

                    dict.update(dict_, temp_dict)

                # else: continue
                # break

        elif card['card_type'] == 9:
            temp_dict_ = {
                # 'id':cgList['mblog']['id'],
                # 以帖子id为key，然后记录每个帖子下的相关信息
                card['mblog']['id']: {
                    'mid': card['mblog']['mid'],
                    'text': card['mblog']['text'],
                    'reposts_count': card['mblog']['reposts_count'],
                    'attitudes_count': card['mblog']['attitudes_count'],
                    'comments_count': card['mblog']['comments_count'],
                    'comments_details': {},
                    'created_at': card['mblog']['created_at'],
                    'status_province': card['mblog']['status_province'],
                    'user_id': card['mblog']['user']['id'],
                    'screen_name': card['mblog']['user']['screen_name']
                }

            }

            dict.update(dict_, temp_dict_)

        # else:
        # continue

        # print(card['card_type'])
        # print(type(card['card_type']))

    # print('cards',type(cards))
    # print('resData',type(resData))

    # print(cards)

    # 行缩进和键值排序
    # json_dict = json.dumps(dict_, indent=4, sort_keys=True, ensure_ascii=False)
    # print(json_dict)



    ## print(json.dumps(dict_, indent=4, sort_keys=True, ensure_ascii=False))

    ## print(type(dict_))

    # 根据每个帖子的id和mid爬取每个帖子下面的具体评论信息
    # 首先遍历已经爬取到的字典中的key，然后获取key中value中的mid，判断是否为空

    # 全部帖子的id数据以及评论数据


    for inD in dict_.keys():
        id = inD
        mid = dict_[id]['mid']
        comment_dict = {}
        comments_count = dict_[id]['comments_count']
        max_id = ""
        while True:
            if max_id == "":
                comments_url = f'https://m.weibo.cn/comments/hotflow?id={id}&mid={mid}&max_id_type=0'

            else:
                comments_url = f'https://m.weibo.cn/comments/hotflow?id={id}&mid={mid}&max_id={max_id}&max_id_type=0'

            comments_res = requests.get(url=comments_url, headers=headers)
            # 出错，解决出错内容   以b'为开始'结束

            print('出错response:',comments_res)
            print('出错responsetype:',type(comments_res))
            print('出错url：',comments_url)
            print((comments_res.status_code))
            print(('内容',comments_res.content))
            print('url',url)
            # comments_res = comments_res.content.decode('utf-8')
            # print("处理后：",comments_res)
            comments_ress = filter_response(comments_res)
            # print('出错responsestype:', type(comments_ress))
            # 如果为none，跳过
            print('过滤后的数据',comments_ress)
            if comments_ress is not None:
                comments_res = comments_ress.content.decode('utf-8')
                comments = json.loads(comments_res)
                if comments['ok'] == 0:
                    break
                print('max_id', max_id)
                print('max_id_type', type(max_id))
                for comments_data in comments['data']['data']:
                    temp_comment_dict = {
                        comments_data['user']['id']: {
                            '帖子id': id,
                            'comment_time': comments_data['created_at'],
                            'comment_text': comments_data['text'],
                            'comment_id': comments_data['user']['id'],
                            'comment_name': comments_data['user']['screen_name'],
                            'comment_source': comments_data['source']
                        }
                    }
                    dict.update(comment_dict, temp_comment_dict)
                    # dict_[id]['comments_details'].update({
                    #     comments_data['user']['id']:temp_comment_dict
                    # })

                    # dict.update(comment_dict,temp_comment_dict)

                    # json_dict_2 = json.dumps(comment_dict, indent=4, sort_keys=True, ensure_ascii=False)
                    # print('评论数据',json_dict_2)
                    # print('评论数据：',comment_dict)
                com_d = json.dumps(comment_dict, indent=4, sort_keys=True, ensure_ascii=False)
                ##print("评论数据：", com_d)

                max_id = comments['data']['max_id']
                if max_id == 0:
                    break

                time.sleep(2)
                print('获取后的max_id',max_id)
            else:
                break
            # comments = json.loads(comments_res)


        # 将获取到的数据添加进原来的字典中
        dict.update(dict_[id]['comments_details'], comment_dict)
    dict.update(result_dict,dict_)
    print('每次的url',url)
    print('page：',page)
    # 设置跳出while条件
    if page == 0 or page == limit:
        break






    #
    # comment_url = f'https://m.weibo.cn/comments/hotflow?id={id}&mid={mid}&max_id_type=0'
    # c_url = f'https://m.weibo.cn/detail/{id}'
    # print(comment_url)
    # # 如果评论不为0，获取评论数据
    # comments_res = requests.get(url=comment_url,headers=headers)
    # tt = comments_res.json()
    # print(tt)
    # # comments_list = comments_res['data']['data']
    # # print('评论列表',comments_list)

# 将爬取到的评论数据与帖子数据进行合并
# for t in dict_.keys():
#     id = t
#     for com_det in comment_dict.keys():
#

json_dict = json.dumps(result_dict, indent=4, sort_keys=True, ensure_ascii=False)
print('最终数据',json_dict)
#
with open(f"{keyword}-result.json", "w",encoding="utf-8") as f:
    json.dump(result_dict, f,ensure_ascii=False, indent=4)



# todo 将json格式的数据提取出来清洗，然后做词云图，然后进行情感分析
