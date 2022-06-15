# 22.06.15 Semi-Project 브레인스토밍

<aside>
🔑 세미프로젝트 전 레퍼런스 및 사용용도 정하기
레퍼런스 : 나이키 
사용용도 : 신발쇼핑몰
부트스트랩 테마 선정

</aside>

## 1. 레퍼런스 : 나이키

![nike.png](22%2006%2015%20Semi-Project%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%89%E1%85%B3%E1%84%90%E1%85%A9%E1%84%86%E1%85%B5%E1%86%BC%200d2c23e573bb42a480c06ab8f3543dc7/nike.png)

- 선정 이유
    - 그림과 같이 왼쪽단에 카테고리 분류 등 다양한 분류를 체크박스를 통해 구현 가능
    - 심플한 UI
    - 세미프로젝트인만큼 전형적인 주제를 가지고 실질적인 코딩을 통한 이벤트 구현 및 화면 구성, 데이터베이스 연동에 초점을 맞추어 진행한다.

## 2. 사용용도 : 신발 쇼핑몰

- 선정이유
    - 앞서 레퍼런스의 선정이유와 동일하게 전형적인 주제를 가지고 구현과 실행에 중점을 둔다.

## 3. 부트스트랩 선정 리스트

[Fashion Shop | Home](https://www.templatebazaar.in/demo/fashion_shop/index.html)

- 메인화면과 세부 제품 페이지 화면 배치가 레퍼런스와 비슷

---

[eStore - Free Bootstrap eCommerce Template - GrayGrids](https://graygrids.com/templates/estore-free-bootstrap-ecommerce-template/)

- 유료 풀버전과 무료 라이트 버전이 있어 라이트 버전이 어디까지 제공하는지 확인 필요

---

[W3.CSS Template](https://www.w3schools.com/w3css/tryw3css_templates_clothing_store.htm)

- W3schools.com에서 제공하는 쇼핑몰 템플릿

---

[MultiShop - Online Shop Website Template](https://demo.htmlcodex.com/1479/online-shop-website-template/)

- 메인화면은 조금 복잡하고 번잡하지만, Shop페이지에 레퍼런스로 생각 했던 체크박스로 상품들을 분류하는 파트가 이미 존재, 응용하기 좋을 듯

## 3. 부트스트랩 선정

[Fashion Shop | Home](https://www.templatebazaar.in/demo/fashion_shop/index.html)

### 선정이유 : 다양한 페이지들이 이미 구성되어있어 응용하기 편리, UI디자인이 좋다.

## 4. 선정한 부트스트랩 수정 및 개선사항 나열

1. Contact Us - Map 설정을 멀티캠퍼스 선릉으로 설정
2. Main Menu 중 Blog Grid View Left 에서 왼쪽 menu의 tag 기능으로 물품 검색
3. Register 페이지에 JavaScript로 조건 추가하기
4. main page 상단에 mail 클릭시 바로 메일 보낼 수 있도록 제작 (참고 사이트 : https://frontcode.tistory.com/33)
1. 메인페이지 footer는 간단하게!
2. 제품별 review 저장하는 table이 필요
3. navbar search 기능 수업에서 배운내용으로 구현
4. 상품페이지의 Related Product를 띄우는 기준 설정이 필요
5. 상품 선택시 detail 페이지 구현
6. 샵페이지를 레퍼런스에서 생각했던 체크박스 부분은 다른 무료 부트스트랩의 소스를 이용
7. 결제 페이지에서 우편번호 API와 결제 API 사용
8. contact 페이지에 있는 이메일 또는 주소 클릭시 클립보드 복사(참고 사이트 : http://b1ix.net/364)
9. 물품 리스트 아래에 있는 페이지 넘김 버튼 제작 할것인지(<12345>)
10. 메인페이지 및 상단메뉴, 제품의 카테고리 등 간소화, 생략 할 수 있는곳 합의해서 생략

## 5. ERD 구성

![Shoes shop ERD.png](22%2006%2015%20Semi-Project%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%89%E1%85%B3%E1%84%90%E1%85%A9%E1%84%86%E1%85%B5%E1%86%BC%200d2c23e573bb42a480c06ab8f3543dc7/Shoes_shop_ERD.png)

### 논의 및 쟁점

- 신발의 SIZE를 어떻게 테이블을 따로 구성하여 연결 시킬것인가
- 테이블의 갯수와 규모가 일주일의 시간내에 구성할 수 있는 범주인가