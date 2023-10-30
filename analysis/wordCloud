import jieba
from matplotlib import pyplot as plt
from wordcloud import WordCloud
from PIL import Image
import numpy as np


# 设置绘制所用到的字体
font = r'C:\Windows\Fonts\FZSTK.TTF'


def tcg(texts):
    cut = jieba.cut(texts)
    string = ' '.join(cut)
    return string

text = (open('cleard.txt','r',encoding='utf-8')).read()
string = tcg(text)

# todo 将背景图绘制出来，使词云图在背景图的内容中展示
img = Image.open('bbg.jpg')
#将图片转换为数组
img_array = np.array(img)
print(img_array)
#设置停止词
stopword=['']

wc = WordCloud(
    background_color='white',
    width=1000,
    height=800,
    mask=img_array,
    font_path=font,
    stopwords=stopword
)

#绘制词云图
wc.generate_from_text(string)

plt.imshow(wc)
plt.axis('off')
plt.show()
#保存图片
# wc.to_file(path+r'\beautifulcloud.png')
