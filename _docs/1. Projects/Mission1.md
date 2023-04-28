---
title: Fetching Public Wifi List(web)
category: Projects
order: 3
---

## Scenario
**1.** Design data using ERD.

**2.** Implement a web application by utilizing an open API provided by Seoul City (Public WiFi List - [https://data.seoul.go.kr/dataList/OA-20883/S/1/datasetView.do](https://data.seoul.go.kr/dataList/OA-20883/S/1/datasetView.do)).

**3.** Develop a dynamic web service (w/o Spring) based on Java (JSP)

---
## Features
**1.** Implement the "fetch public WiFi list" function.

**2.** Display the 20 nearest public WiFi locations based on the user's current location.

**3.** Store inquiry history in the database (MariaDB).

**4.** Provide detailed information for each WiFi location.

**5.** Offer bookmarking and naming services (CRUD).

---

## DB Entities

|GROUP_TABLEㅤㅤ||||
|--|--|--|--|
|pk|GR_SEQ_NO|int(11)|NOT NULL|
||GR_NM|varchar(100)|NOT NULL|
||GR_PRIORITY|int(11)|NOT NULL|
||GR_DT_RGISTERED|timestamp|NULL|
||GR_DT_MDFIED|timestamp|NULL|

|BOOKMARK_TABLE||||
|--|--|--|--|
|pk|BM_SEQ_NO|int(11)|NOT NULL|
||GR_NM|varchar(100)|NOT NULL|
||WIFI_NM|varchar(100)|NOT NULL|
||WIFI_ID|char(20)|NOT NULL|
||BM_DT_RGISTERED|timestamp|NOT NULL|

|INQUIRY_HISTORY||||
|--|--|--|--|
|pk|HS_SEQ_NO|int(11)|NOT NULL|
||LAT_INQUIRED|varchar(100)|NOT NULL|
||LNT_INQUIRED|varchar(100)|NOT NULL|
||DT_REGISTRED|char(20)|NOT NULL|



---
## How did i Implement?

**1.** Seoul Wifi List API

**2.** Using servlet

**3.** DataBase(MariaDB) connection

**4.** JSP
 
---
## Challenges i've faced.

-

---
## Things need to be complemented

-

---
## Lessons

-

---
## Repository 

[**[Move to Repository]**](https://github.com/HyunsooZo/zerobase-Mission1)

