# 20151120
    
    먼지지수 확인하기
    
    import urllib2 # extensible library for opening URLs
import time
from datetime import datetime, timedelta
import json
import requests
import sys

from lxml.html import parse, fromstring # processing XML and HTML

url = 'http://www.kma.go.kr/wid/queryDFSRSS.jsp?zone=2823759100'
url_local ="http://127.0.0.1:4242/api/put"

temp=[]


def insert(metric_name, tag_site, value):
    data={
        "metric":metric_name,
        "timestamp":time.time(),
        "value":value,
        "tags":{
            "site":tag_site
        }
                                                               
    }
    ret = requests.post(url_local, data=json.dumps(data))
    time.sleep(1)

def temp_process(xml):
        for  elt in xml.getiterator("temp"):    # getting temp tag 
                temp_val = elt.text
        print temp_val
        insert('dust','incheon',temp_val)
        insert('dust','seoul',17)


if __name__ == '__main__':
        page = urllib2.urlopen(url).read()
        print page

        xml_raw = fromstring(page)

        temp_process(xml_raw)

