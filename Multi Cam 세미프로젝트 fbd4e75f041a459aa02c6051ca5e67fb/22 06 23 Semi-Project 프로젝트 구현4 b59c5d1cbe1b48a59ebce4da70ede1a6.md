# 22.06.23 Semi-Project 프로젝트 구현4

<aside>
🔑 * 프로젝트 진행에서의 핵심적인 문제 파악 후 그것들을 수정
1. AJAX로 Thymeleaf for문이 돌아간 값들을 다시 회수하여 비동기방식 반응을 시킬 수 있는지..?
2. 결제 페이지에서 주문상세의 input 정보들(주문번호,pid 등...)을 가져오는 것 까지는 성공을 했지만 한꺼번에 여러가지의 상품을 주문할 시 다수의 form이 생성되며, 그 다수의 form을 한번에 submit이 가능한지..
3. 결제 페이지에서 결제를 성공했을시 주문 정보(주문자아이디,배송지,수령인...등등)와 주문 상세정보 (주문정보, 제품번호.. 등등)을 하나의 결제페이지에서 한꺼번에 form에 담아 submit이 가능한지..

</aside>

## I) 프로젝트 진행에서의 핵심적인 문제 파악 후 그것들을 수정

---

## 1. AJAX로 Thymeleaf for문이 돌아간 값들을 다시 회수하여 비동기방식 반응을 시킬 수 있는지..?

- 장바구니 수량과 금액에 대한 개별 장바구니 리스트의 금액 합계
- 전체적인 장바구니 금액 합계
- 시도 :
    - 배열에 넣어 배열을 넣은 변수를 AJAX Controller에 데이터로 보내서 AJAX controller에서 for문으로 처리 후 다시 데이터 반환(실패)
    - th:id 값으로 변수 설정하여 아이디 값 지정후 아이디값당 수량*갯수(실패)

해답 : input아이디를 각각의 cart id로 받아와서 그 input id를 JSON으로 받아온다.

## 2. 결제 페이지에서 주문상세의 input 정보들(주문번호,pid 등...)을 가져오는 것 까지는 성공을 했지만 한꺼번에 여러가지의 상품을 주문할 시 다수의 form이 생성되며, 그 다수의 form을 한번에 submit이 가능한지..

- 주문 상세인풋을 th:each로 가져오기 때문에 상품 갯수에 따라서 여러가지의 제품 상세목록이 생성될 가능성이 있다.
- 시도:
    - 결제 성공페이지 이후 그 페이지에서 document가 열릴시 바로 폼보내면서 redirect로 억지로 다시 성공 페이지로 돌려보냈더니 첫번째 값만 반복적으로 데이터베이스안으로 들어감
    - .class로 뭉쳐서 form submit을 한꺼번에 보냈을 시에도 첫번째 값만 들어감

## 3. 결제 페이지에서 결제를 성공했을시 주문 정보(주문자아이디,배송지,수령인...등등)와 주문 상세정보 (주문정보, 제품번호.. 등등)을 하나의 결제페이지에서 한꺼번에 form에 담아 submit이 가능한지..

- 결제 상품이 4개라고 생각하면 주문정보에대한 폼 1개와 각각의 상품에 대한 주문 상세가 4개의 폼으로 나올텐데, 그것들을 한번에 보낼 수 있는지..?
- 시도:
    - form serialize()를 통한 form 합치기 (실패)
    - form의 action을 통일 후 각각의 데이터들을 따로 VO로 받아 각각의 값들을 데이터베이스로 넣어주기(실패)
    - Controller단의 buyimpl 에서 처리후 buydetail으로 넘겨주고 그 후 또 처리(실패)
    - return값이 없는 void함수를 Controller단에서 사용(실패)
    

해답: 일단 결제 페이지에서는 buy정보만 보내준다.

---

## II) 새롭게 만든 기능

### 1. Session 이용해서 cart연동시키기

- 로그인이 되지 않은 상태에서 카트를 누르면 로그인 페이지로 이동

### 2. Checkout 페이지에서 주문정보 (주문자 아이디, 배송지 등등) 만 보내준다

> 주문상세정보는 그다음 페이지에서 진행한다.
> 

### 3. 결제 완료후 success페이지로 이동 후 화면이 출력되자마자 주문상세정보 데이터 전송

### 4. 데이터 전송 후 success2페이지로 이동하고 주문상세 데이터를 뿌려준다.

> 직접 보는 고객들에게는 페이지가 한번넘어간것처럼 보인다.
> 

---

## III) 해결 못한 문제

1. 결제 완료 후 buyimpl로 넘어가서 주문정보를 데이터에 입력한 후 success 페이지에서 document가 준비될시 바로 buy_detailimpl로 이동하여 주문 상세정보를 데이터 베이스에 넣는다.
    1. 주문 상세정보의 최상단의 한가지의 폼만 전송된다.
2. 장바구니의 주문 합계
    1. 전체적인 장바구니의 정보들을 JSONArray를 이용하여 Controller단에서 Model로 보내 Script단에서 JSON으로 AJAXController에 보낸다.
    - 메이븐 추가
        1. 오류 발생
        
    
    ```java
    <!-- JASON ArrayList-->
    		<dependency>
    			<groupId>net.sf.json-lib</groupId>
    			<artifactId>json-lib</artifactId>
    			<version>2.4</version>
    			<classifier>jdk15</classifier>
    		</dependency>
    //controller단
    m.addAttribute("jsonList",jsonArray.fromObject(clist));
    m.addAttribute("jsonList",jsonArray.fromObject(clist));
    //에러 항목
    java.lang.NoClassDefFoundError: net/sf/json/JSONArray
    ```
    
    > Question 키워드 : List, Form, submit
    > 

## III) 버튼을 누르면 바디안에 몇번째 tr인지 찾는다.

jQuery trigger