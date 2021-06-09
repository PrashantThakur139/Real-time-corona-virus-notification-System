# Real-time-corona-virus-notification-System
This project will give you the real-time update about the number of new cases, deaths, and the recovered cases of coronavirus within regular interval of time in particular area. It will also provide information about how many vaccination is done all over country 

CODE for implementation of this project:-


from plyer import notification
import requests
from bs4 import BeautifulSoup
import time

def notifyMe(title, message):
    notification.notify(
        title = title,
        message =  message,
        app_icon ="E://docs//final project//New folder//virus.ico",
        timeout = 30
    )
def notify_me(title, message):
    
    notification.notify(
         title = title,
         message = message,
         app_icon = "E://docs//final project//New folder//virus.ico",
         timeout = 10,
         )
def getData(url):
    r = requests.get(url)
    return r.text
if _name_ == "_main_":
     while True:      
        myHtml = getData('https://www.mohfw.gov.in/')
        
        data = BeautifulSoup(myHtml, 'html.parser')
        
        vaccine = data.find("div", {'class': 'fullbol'}).find("span",{'class': 'coviddata'}).get_text()
        print(vaccine)

       
        myHtmlData = getData('https://www.medtalks.in/live-corona-counter-india')


        soup = BeautifulSoup(myHtmlData, 'html.parser')
        #print (soup)
        
        myDataStr = ""
        for tr in soup.find_all('tbody')[0].find_all('tr'):
            myDataStr += tr.get_text()
        myDataStr = myDataStr[1:]
    

        itemlist = myDataStr.split("\n\n")
        #print(myDataStr)

        states = ['Delhi  ( दिल्ली )','Maharashtra ( महाराष्ट्र )','Himachal Pradesh ( हिमाचल प्रदेश )']
        for item in itemlist[0:62]:
            
            dataList = item.split('\n')
            print(dataList)
            if dataList[0] in states:
                print(dataList)
                nTitle='Cases of covid-19'
                nText=f"{dataList[0]}:cases:{dataList[1]}:active:{dataList[2]}"
                notifyMe(nText,nTitle)
                Title='deaths of covid-19'
                Text=f"{dataList[0]}:deaths:{dataList[3]}"
                notifyMe(Text,Title)
            
        notify_title = "COVID-19 Status (vaccination)"
        notify_text = f"vaccine : {vaccine}" 
        notify_me(notify_title, notify_text)
        time.sleep(3600)
