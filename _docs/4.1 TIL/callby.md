---
title: Call By Value/Reference
category: TIL
order: 10
---

###  Call by Value/Reference

<div class="content-box">
메서드를 호출할 때 파라미터를 전달하는 방법에는 두 가지가 있다.
</div>

![exmpl](https://drive.google.com/uc?id=1c1IZWIHI-f5CRV707K-oIGuUCLV7jDMe)

##### Call by Value
- `Call by Value` 는 메서드를 호출할 때 값을 넘겨주기 때문에 `Pass by Value` 라고도 부른다

- 메서드를 호출하는 호출자 (`Caller`) 의 변수와 호출 당하는 수신자 (`Callee`) 의 파라미터는 복사된 서로 다른 변수이다.

- 값만을 전달하기 때문에 수신자의 파라미터를 수정해도 호출자의 변수에는 아무런 영향이 없다.

#####  Call by Reference

- `Call by Reference` 는 참조 (주소) 를 직접 전달하며 `Pass By Reference` 라고도 부른다.

참조를 직접 넘기기 때문에 호출자의 변수와 수신자의 파라미터는 완전히 동일한 변수이다.

메서드 내에서 파라미터를 수정하면 그대로 원본 변수에도 반영된다.
