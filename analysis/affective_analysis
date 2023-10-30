
import matplotlib.pyplot as plt
from snownlp import SnowNLP

#  根据文件路径提取出txt文件中每一行代码
file_path = 'cleard.txt'
# 定义字典，使用字典来存储每条内容的情感分析结果
sentiments = {'positive': 0, 'negative': 0, 'neutral': 0}

# 使用snowNLP对提取出的每一行内容进行情感分析
with open(file_path, 'r', encoding='utf-8') as file:
    # 逐行读取文件内容
    lines = file.readlines()
    for line in lines:
        # 去除每一行开始的空数据
        line = line.strip()
        # 使用Snownlp进行情感分析
        sentiment_score = SnowNLP(line).sentiments
        print('每一行的数据', line)
        print('每一行的数据的得分', sentiment_score)
        if sentiment_score > 0.55:
            sentiments['positive'] += 1
        elif sentiment_score < 0.45:
            sentiments['negative'] += 1
        else:
            sentiments['neutral'] += 1

labels = list(sentiments.keys())
sizes = list(sentiments.values())

# 绘制饼状图，将情感得分绘制
fig, ax = plt.subplots()
ax.pie(sizes, labels=labels, autopct='%1.1f%%', startangle=90)
ax.axis('equal')
ax.set_title('affective-analysis')

plt.show()




