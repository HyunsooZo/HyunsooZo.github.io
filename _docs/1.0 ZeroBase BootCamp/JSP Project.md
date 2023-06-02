---
title: JSP Project(1)
category: ZeroBase BootCamp
order: 3
---
### Repository and Demo Video

[**[Demo Video Link]**](https://vimeo.com/823358509?share=copy)

[**[Repository Link]**](https://github.com/HyunsooZo/zerobase-Mission1)

### Scenario

추후 작성예정

### Project Tech Stack

<div class="content-box">
- JSP/Servlet 사용<br>
- MariaDB 사용<br>
- tomcat(WAS) 사용<br>
</div>

### Areas for Improvement   

**1.** 
mariadb 외 다른 DB도 사용해볼 것 <br>

**2.** 데이터를 연결, close하는 부분은 공통으로 처리하기.<br>
베이스클래스로 만들어서 상속을 통해서 처리하거나, 유틸리티클래스를 만들어서 공통으로 처리하는 방법 등..
<br>

**3.** 
데이터를 담는 클래스는 ~Model, ~Dto, ~Vo등으로 표시 <br>
비즈니스 서비스를 처리하기 위한 클래스는 ~Service로 표시<br>
강제되는 건 아니지만, 딱보고 알수 있도록 클래스와 패키지 정리 필요.
<br>

**4** wifi정보를 가져오는 부분 가져와서 DB에 저장하는 부분 따로, 가까운 위치에 목록을 가져오는 부분 따로(이건 DB에서 바로) 해서 구현.<br> 
 가까운 위치에서 가져오는 부분은 이미 디비에 있는 데이터에서만 가까운 정보를 보여지도록 처리
 <br>


### Lessons Learned

각종 라이브러리와 JSP,서블릿을 사용하여 외부 api(rest)를 호출하는 법을 배워보았는데 <br>

**JSP,Servlet**
<div class="content-box">
JSP문법이 다소 낯설었으나 기본적인 HTML , JavaScript의 문법을 다듬는데에 도움이 많이 되었던 것 같다. <br>
또한 xml이 아닌 어노테이션을 이용한 서블릿 컨텍스트지정 및 호출이 매우 편리하다고 느껴졌다. 
</div>

**MariaDB**
<div class="content-box">
DB를 연동하여 CRUD 쿼리를 보내는 부분에서 처음에 많이 헤맸지만 기본적인 툴을 만들어 놓으면 이후<br> 
사용에 있어서는 크게 어려움이 있지는 않았다. <br>
</div>

**Rest api(seoul open api)**
<div class="content-box">
rest api를 호출하여 받아온 Json데이터를 정제하는 과정에서 자바의 자료구조를 활용할 수 있다는 점이 흥미로웠다.<br> 
</div>

**Tomcat**
<div class="content-box">
첫 웹서버 연동 시 PC환경 설정하는데에 조금 시간이 걸렸지만, 다시 해본다면 좀더 빠르게 설정할 수 있을 것 같다. 
</div>
