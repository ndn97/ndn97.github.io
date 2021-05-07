---
layout: post
title:  Python Vaccine API Implementation
tags: python3 linux API 
---

Currently Govt has open sourced covid vaccine availability API's in India as given [here](https://apisetu.gov.in/public/marketplace/api/cowin), Vaccine slots are limited and  difficult to know when those slots gets opened.
Here is simple bot created to monitor the vaccine slots and notify interested users when those slots are available.
   
    #!/usr/bin/env python3
    import json,urllib3,datetime,time,logging,telegram,configparser

    config = configparser.ConfigParser()
    config.read_file(open('api.conf'))
    weburl = config.get('cowin', 'url')
    TOKEN = config.get('telegram', 'token')
    CHAT_ID = config.get('telegram', 'chat_id')
    x=datetime.datetime.now()

    day=x.strftime("%d-%m-%Y")
    weburl+=day
    http = urllib3.PoolManager()
    logging.basicConfig(filename='./vaccine.log',level=logging.INFO,format='%(asctime)s %(message)s')
    bot = telegram.Bot(token=TOKEN)

    while True:
        found=0
        try:
            response = http.request('GET', weburl)
            json_data = json.loads(response.data)
        except Exception as ex:
            logging.info("API is not responding!")
            time.sleep(10)
            continue

        for x in json_data['centers']:
            for y in x['sessions']:
                if y['min_age_limit'] == 18 and y['available_capacity'] > 0:
                    #print(y)
                    message = '18+ vaccine available in #BBMP: '+ str(x['pincode']) +' '+ str(y['date'])+ ' ' + x['name'] +' Avail slots: '+ str(y['available_capacity'])
                    logging.info(message)
                    bot.sendMessage(chat_id = CHAT_ID, text =message)
                    found=1
                    break
                else:
                    continue
                break
        if (found == 0):
            time.sleep(4)
        else:
            continue
 API is  enforcing 100 calls/5 mins rate limit per IP, which translates to 1 call/3 seconds,hence after every iteration, the bot sleeps for 4 seconds
 Incase slots are open ( found=1), need to validate and alert without this 4 seconds delay, hence utilizes double break points and skips the 4 seconds delay with found=1 flag
 
 Example snapshot of the telegram alerts is as follows:
 ![Telegram Notification Snapshot](/assets/screenshots/telegram-vaccine-slot.jpg)
 
 

