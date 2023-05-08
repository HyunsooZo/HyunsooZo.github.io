---
title: Project_1(Web App Using OpenAPi)
category: ZeroBase BootCamp
order: 1
---
### Demo Video

[Demo Video Link](https://vimeo.com/823358509?share=copy)


### Scenario

**1.** Design data using ERD.

**2.** Implement a web application by utilizing an open API provided by Seoul City (Public WiFi List - [link](https://data.seoul.go.kr/dataList/OA-20883/S/1/datasetView.do)).

**3.** Develop a dynamic web service (w/o Spring) based on Java (JSP)


### Features

**1.** Implement the "fetch public WiFi list" function using Seoul Open API.

**2.** Display the nearest public WiFi locations (up to 20) based on the user's current location.

**3.** Store inquiry history in the database (MariaDB).

**4.** Provide detailed information for each WiFi location.

**5.** Offer bookmarking and naming services (CRUD).


### DB Entities

**GROUP_TABLE**

|pk|Column|Type|NULL?|
|--|--|--|--|
|✓|GR_SEQ_NO|int(11)|NOT NULL|
||GR_NM|varchar(100)|NOT NULL|
||GR_PRIORITY|int(11)|NOT NULL|
||GR_DT_RGISTERED|timestamp|NULL|
||GR_DT_MDFIED|timestamp|NULL|

**BOOKMARK_TABLE**

|pk|Column|Type|NULL?|
|--|--|--|--|
|✓|BM_SEQ_NO|int(11)|NOT NULL|
||GR_NM|varchar(100)|NOT NULL|
||WIFI_NM|varchar(100)|NOT NULL|
||WIFI_ID|char(20)|NOT NULL|
||BM_DT_RGISTERED|timestamp|NOT NULL|

**INQUIRY_HISTORY**

|pk|Column|Type|NULL?|
|--|--|--|--|
|✓|HS_SEQ_NO|int(11)|NOT NULL|
||LAT_INQUIRED|varchar(100)|NOT NULL|
||LNT_INQUIRED|varchar(100)|NOT NULL|
||DT_REGISTRED|char(20)|NOT NULL|


### How did i Implement?

**1.** Seoul Wifi List API

**2.** Using servlet

**3.** DataBase(MariaDB) connection

**4.** JSP
 

### Challenges i've faced.

-


### Things need to be complemented

-


### Lessons

-


### Repository 

[**[Move to Repository]**](https://github.com/HyunsooZo/zerobase-Mission1)

