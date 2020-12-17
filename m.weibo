import requests, os, time, random
from pyquery import PyQuery as pq
from opencc import OpenCC        # OpenCC為簡體轉繁體的python套件

# 定義將簡體轉為繁體的變數
cc = OpenCC('s2hk')

agent = 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) \
        Chrome/86.0.4240.111 Safari/537.36'
cookie = 'SUB=_2AkMoz4C_f8NxqwJRmPwdy2zibIVzzQDEieKek3FkJRMxHRl-yT9jqkostRB6A0-uUHe1CK5RKu2bSVDF1hSQg1Fi0KWe; \
          SUBP=0033WrSXqPxfM72-Ws9jqgMF55529P9D9W5lCVhHvhRjPP7ys9zgIjOO; wb_view_log=1536*8641.25; \
          SINAGLOBAL=5487027685113.079.1603473292019; _s_tentry=-; Apache=2873028647471.1763.1603548398528; \
          ULV=1603548398554:2:2:2:2873028647471.1763.1603548398528:1603473292024'
headers = {'User-Agent': agent, 'cookie':cookie}


def download_onepage(url):
#爬取一頁的內容

    #針對指定的網頁發出請求
    res = requests.get(url=url , headers=headers)
    res.encoding = 'utf-8'
    time.sleep(random.uniform(5,8))
    #解析網站的架構，本身是json格式，這裡就不用BeautifulSoup
    res_json = res.json()

    #找出text的對應內容
    cards = res_json["data"]["cards"]

    #內文
    for i in cards:
        if i['card_type'] == 9:                   #9是微博card，11是推薦有趣的
            mblog = i["mblog"]
            content = pq(mblog["text"]).text()    #pq只保留文本
            date = mblog["created_at"]
            final_content = cc.convert(content)
            print(date, final_content)
            #將博客內容存成txt檔案
            with open("./Diary/weibodiary.txt", mode="a", encoding="utf-8") as f:
                f.write(','.join([date, final_content]) + '\n')
        else:
            pass

url = 'https://m.weibo.cn/api/container/getIndex?type=uid&value=3389738692&containerid=1076033389738692&page={}'

page = 1
while page <= 210:
    download_onepage(url.format(page))    
    time.sleep(random.uniform(8,14))
    page += 1
