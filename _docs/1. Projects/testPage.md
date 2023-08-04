---
title: Let's Grab a Lunch
category: Projects
order: 1
---

### 프로젝트 README

##### 프로젝트 개요
<div class="content-box"> 
이 프로젝트는 회사 근처 사람들이 점심 약속을 잡고 리뷰할 수 있는 <br>
커뮤니티를 위한 api를 만들어보았다.
</div>

##### 사용기술 
- [x] Spring Boot<br/>
- [x] Hibernate<br/>
- [x] MariaDB<br/>
- [x] Spring Security<br/>
- [x] Spring Scheduling<br/>
- [x] JWT (HMAC512알고리즘)<br/>
- [x] 외부 API 연동<br/>
&nbsp;&nbsp; - Geocode API : 공공데이터<br/>
&nbsp;&nbsp; - SMS 전송 API : 네이버클라우드<br/>
&nbsp;&nbsp; - OCR API     : 네이버클로바<br/>

##### API 목록 및 기능

|API|기능|
|--|--|
|주변회원 조회 api|- 내 회사 Nkm 반경 이내 회원조회<br>- 거리가 가까운순으로 회원정렬 및 회사명으로 검색 가능|
|점심약속-조회/생성/수정/삭제 + 승낙/거절 api|- 생성 : 사용자는 본인의 주변회원 리스트에 조회된 회원에게 시간/장소/메뉴를 적어 점심약속신청 가능.<br>- 조회 : 사용자는 본인에게 신청된 점심약속 리스트 조회가능.<br>- 수정 : 점심약속 수락/거절 전 약속신청인은 생성된 점심약속 수정가능<br>- 취소/삭제 : 점심약속 1시간전 두 사용자는 해당 약속을 취소 가능./점심약속을 신청을 한 사용자는 약속을 삭제 가능.<br>- 승낙/거절 : 점심약속을 받은 피신청자는 점심약속을 승낙/거절 가능.<br>- 외부 api 연동 : 생성된(신청된) 점심약속 내용을 피신청자에게 문자메세지로 전송|
|점심약속 상태 업데이트 api(스케줄링)|- 1분 단위로 스케쥴링 하여 점심약속시간이 지난 `PROPOSED`요청 상태의 점심약속 상태 `EXPIRED`로 업데이트.<br>- 1분 단위로 스케줄링 하여 점심약속시간이 지난 `ACCEPTED`수락 상태의 점심약속 상태 `COMPLETED`로 업데이트|
|사용자 평가 api|- 약속상태가 `COMPLETED` 완료된 상대의 프로필에 사용자는 서로에게 별점을 매기고 댓글작성 가능|
|회원가입 api|- 사용자 회원가입은 회원정보 직접입력을 통해 가능<br>- 회원비밀번호는 `BCryptPasswordEncoder`를 통해 암호화<br>- *추가구현(외부api)-> 명함OCR가입*|
|로그인 api|- 사용자는 아이디/비밀번호로 로그인 가능<br>- 비밀번호 복호화 -> 일치 시 토큰 발행|

##### ERD

![ERD](https://drive.google.com/uc?id=1ygug3YzOmOSk4R3MUAgpUQ_ecEPSPXap)


###
