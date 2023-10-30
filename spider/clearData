import json
import re

with open('上海-result.json','r',encoding='utf-8') as file:
    data = json.load(file)


# 先获取所有的评论信息，然后将评论信息保存为字典格式，然后将字典中清洗，只保留文本，存入列表中

comments_dict = {}

for iK in data.keys():
    if data[iK]['comments_count']:
        # print('评论数为：',data[iK]['comments_count'])
        temp_comm = {}
        for cK in data[iK]['comments_details'].keys():
            # print("ck是",cK)
            # 清洗数据，只保留文本
            text = data[iK]['comments_details'][cK]['comment_text']
            # 删除HTML标签
            text = re.sub('<.*?>', '', text)
            # 删除表情符号
            text = re.sub('[^ \u4e00-\u9fa5a-zA-Z0-9]+', '', text)
            # 去除多余的空格
            text = re.sub('\s+', ' ', text).strip()
            # 除去数字
            text = re.sub(r'\d+', '', text)
            # 剔除清洗后的内容为空的情况
            if text == '':
                # print('ck',cK)
                continue
            temp_comments_dict = {
                cK:text
            }
            dict.update(temp_comm,temp_comments_dict)


    dict.update(comments_dict,temp_comm)

    # # 拿一个进行测试
    # if iK ==4962290258150117:
    #     break


# 去除重复元素
comments_set = set(comments_dict.values())



# comments_list = []
#
# for cD in comments_dict.keys():
#     comments_list.append(comments_dict[cD])
#
# for key, value in comments_dict.items():
#     # 如果值为空字符串，则输出对应的键
#     if value == '':
#         print('key',key)


print('字典元素',comments_dict.values())
print('集合元素',comments_set)
# comments_dict_2 = json.dumps(comments_dict, indent=4, sort_keys=True, ensure_ascii=False)
#
# print('提取后的评论数据',comments_dict_2)

with open('cleard.txt','w',encoding='utf-8') as f:
    for item in comments_set:
        f.write(item + '\n')

