# 22.06.20 Semi-Project 프로젝트 구현1

<aside>
🔑 각자 맡은 역할내의 페이지 구성하기
1. 카트 페이지 기능 및 페이지 구성하기.
2. 주문 페이지 기능 및 페이지 구성하기.

</aside>

## 1. 카트 페이지 기능 및 페이지 구성

### - CartController.java 생성 및 경로 설정 (login page또는 login환경이 설정되어 있지 않기에 모든 id값의 파라미터는(”id02”)로 통일)

```java
package com.multi.controller;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

import com.multi.biz.Addr_listBiz;
import com.multi.biz.Buy_detailBiz;
import com.multi.biz.CartBiz;
import com.multi.vo.Addr_listVO;
import com.multi.vo.CartVO;

@Controller
@RequestMapping("/cart")
public class CartController {

	@Autowired
	CartBiz cartbiz;
	
	@Autowired
	Buy_detailBiz bdbiz;
	
	@Autowired
	Addr_listBiz albiz;
	
	@RequestMapping("")
	public String cart(Model m) {
		List<CartVO> clist =null;
		try {
			clist = cartbiz.uidselect("id02");
			int total = cartbiz.gettotal("id02");
			m.addAttribute("center", "cart/cart");
			m.addAttribute("clist", clist);
			m.addAttribute("total", total);
		} catch (Exception e) {
			e.printStackTrace();
		}
		
		return "index";
	}
	
	@RequestMapping("/checkout")
	public String checkout(Model m) { 
		List<CartVO> clist =null;
		Addr_listVO al = null;
		try {
			clist = cartbiz.uidselect("id02");
			int total = cartbiz.gettotal("id02");
			al = albiz.getcustinfo("id02");
			m.addAttribute("center", "cart/checkout");
			m.addAttribute("clist", clist);
			m.addAttribute("total", total);
			m.addAttribute("al", al);
		} catch (Exception e) {
			e.printStackTrace();
		}
		return "index";
	}
	
	
}
```

- CartVO.java 생성자 및 필드 추가 (JOIN에 따른 생성자 및 변수 추가 필요)

```jsx
package com.multi.vo;

import lombok.AllArgsConstructor;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;
import lombok.ToString;

@Setter
@Getter
@AllArgsConstructor
@NoArgsConstructor
@ToString
public class CartVO  {
	private int id;
	private int count;
	private String uid;
	private int pid;
	private int size;
	private String pname;
	private String pprice;
	private String pimg1;
	private int total;
	
	// cart insert, update
	public CartVO(int id, int count, int pid) {
		this.id = id;
		this.count = count;
		this.pid = pid;
	}
	// cart delete
	public CartVO(int id) {
		this.id = id;
	}
}
```

- cartampper.xml 쿼리문 추가 (cart list에 imgname과 pname 등 필요 / total 금액을 나타내기 위함)

```xml
<select id="uidselect" parameterType="String" resultType="cartVO">
		SELECT  c.uid as uid, p.name as pname,p.imgname1 as pimg1,p.price as pprice, c.count as count,c.size as size FROM cart c
		INNER JOIN product p on c.pid=p.id
		WHERE uid = #{id}
	</select>
	<select id="gettotal" parameterType="String" resultType="int">
		SELECT  sum((p.price)*(c.count)) as total FROM cart c
		INNER JOIN product p on c.pid=p.id
		WHERE uid = #{id}
	</select>
```

- [CartMapper.java](http://CartMapper.java)에 해당 함수 추가

```java
public List<CartVO> uidselect(String id) throws Exception;
public int gettotal(String id) throws Exception;
```

- CartBiz.biz에 해당 함수 추가

```java
public List<CartVO> uidselect(String id) throws Exception{
		return dao.uidselect(id);
	}
	public int gettotal(String id) throws Exception{
		return dao.gettotal(id);
	}
```

- cart.html 내용 수정 (thymeleaf 사용하여 이미지 경로 설정 및 Cartlist 보여주기, 금액의 총합 설정 *금액의 총합 설정은 AJAX로 구현할지 고민)
    
    ![cart.png](22%2006%2020%20Semi-Project%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%20%E1%84%80%E1%85%AE%E1%84%92%E1%85%A7%E1%86%AB1%202faebb67ee0e41fea977969687aecfb7/cart.png)
    

## 2.주문 페이지 기능 및 페이지 구성

- Addr_listVO.java수정 (cname, telphone과 같은 커스트 정보를 JOIN으로 가져오기 위함)

```java
package com.multi.vo;

import lombok.AllArgsConstructor;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;
import lombok.ToString;

@Setter
@Getter
@AllArgsConstructor
@NoArgsConstructor
@ToString
public class Addr_listVO {
	private int id;
	private String uid;
	private String addr;
	private String addr_detail;
	private int zip;
	private String cname;
	private String telphone;
	public Addr_listVO(String uid, String addr, String addr_detail, int zip) {
		this.uid = uid;
		this.addr = addr;
		this.addr_detail = addr_detail;
		this.zip = zip;
	}
	public Addr_listVO(int id, String uid, String addr, String addr_detail, int zip) {
		this.id = id;
		this.uid = uid;
		this.addr = addr;
		this.addr_detail = addr_detail;
		this.zip = zip;
	}
}
```

- addr_listmapper.xml 쿼리문 추가 (주문 창에서 고객정보를 가져오기 위함, 리스트중 첫번째 값만 가져온다.)

```xml
<select id="getcustinfo" parameterType="String" resultType="addr_listVO" >
		SELECT al.uid as uid, al.addr as addr, al.addr_detail as addr_detail, al.zip as zip, c.name as cname, c.telphone as telphone FROM addr_list al
		INNER JOIN cust c ON al.uid = c.id
		WHERE uid = #{id}
    order by addr desc limit 1
	</select>
```

- Addr_listmapper.java 쿼리문에 맞는 함수 생성

```java
public Addr_listVO getcustinfo(String id) throws Exception;
```

- Addr_listBiz.java

```java
public Addr_listVO getcustinfo(String id) throws Exception {
		return dao.getcustinfo(id);
	}
```

- checkout.html 생성 및 내용 수정
    - 좌측단에는 기존 고객의 고객정보와 배송정보를 담은 폼을 구성 (Readyonly로 수정 불가)
    - 우측단에는 수령인의 배송정보와 주문 폼을 구성
        - 상세주소 등 주소를 받기 위해 Daum 주소 API 사용
    - 해당 페이지 역시 Cartlist를 받아 띄워준다.
    - 동일하게 합계도 받아온다.

![checkout.png](22%2006%2020%20Semi-Project%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%20%E1%84%80%E1%85%AE%E1%84%92%E1%85%A7%E1%86%AB1%202faebb67ee0e41fea977969687aecfb7/checkout.png)