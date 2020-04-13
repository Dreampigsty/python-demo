import urllib.request
import re
from bs4 import BeautifulSoup
import pandas as pd


def getSoup(url):
    page = urllib.request.urlopen(url)
    html = page.read()
    html = html.decode('utf-8')
    soup = BeautifulSoup(html, 'html.parser')
    return soup

def getContent(soup, key):
    result = []
    tds = soup.find('td', text=re.compile(key)).parent.select("td")
    for td in tds:
        try:
            result.append(trip(td.string))
        except:
            pass
    return result

def getDate(soup):
    result = re.findall(r"\d+", soup.find('h1').string)
    return result[0]+"/"+result[1]+"/"+result[2]

def trip(s):
    return s.replace('\r','').replace('\n','').replace('\t','')

def getData(url):
    soup = getSoup(url)
    content = getContent(soup, "Au99.99")
    content.insert(0, getDate(soup))
    return content

def getHeader(url):
    soup = getSoup(url)
    content = getContent(soup, "合约")
    content.insert(0, "日期")
    return content



if __name__ == '__main__':
    result = []
    base_url = "https://www.sge.com.cn/sjzx/mrhqsj/"
    end = 5147415
    start = 5147414
    header = getHeader(base_url+str(end))
    for i in range(start, end+1):
        try:
            result.append(getData(base_url + str(i)))
        except:
            print(i)
    writerCSV = pd.DataFrame(columns=header, data=result)
    writerCSV.to_csv('./no_fre.csv', encoding='utf-8', index=None)
