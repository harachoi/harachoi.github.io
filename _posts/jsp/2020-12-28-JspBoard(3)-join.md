---
title: JSP 게시판 제작[3] - 회원가입
toc: true
categories:	
    - JSP Board
tags: 
- Java
- JSP
last_modified_at:
---

```
1) 데이터 베이스 만들기(USER 테이블)
2) 회원가입 폼 만들기
3) User 객체 만들기
4) UserDAO 생성
5) 회원가입 기능 동작 페이지 만들기


디렉토리 구조
-------------------------
src
- user
	- User.java
	- UserDAO.java
    
WebContent
- index.jsp
- main.jsp
- join.jsp
- joinAction.jsp
-------------------------
```

## 1) User 테이블 만들기

```
- BBS 데이터베이스 저장소를 만들고
테이블을 만들어주자.

- USER 아이디를 PRIMARY KEY로 설정하자.

- SQL 문

create table USER(
	userID VARCHAR(20) PRIMARY KEY,
	userPassword VARCHAR(20),
	userName VARCAHR(20),
	userGender VARCHAR(20),
	userEmail VARCHAR(50)
);
```

[![image](https://user-images.githubusercontent.com/49560745/98510827-20cb2880-22a7-11eb-893a-0471b594d809.png)](https://user-images.githubusercontent.com/49560745/98510827-20cb2880-22a7-11eb-893a-0471b594d809.png)

## 2) 회원가입 화면 (폼) 만들기

```
1. 테이블을 만들고, 폼형식으로 데이터를 전달해주자.
<form method="post" action="joinAction.jsp">

2. 성별 체크
js 함수로 구현해봤다.

------------------------------------------------------------
function doOpenCheck(chk){
		let obj=document.getElementsByName("userGender");
			for(let i=0;i<obj.length;i++){
				if(obj[i]!=chk){
					obj[i].checked=false;
				}
			}
		}    
------------------------------------------------------------
```

[![image](https://user-images.githubusercontent.com/49560745/98510044-bd8cc680-22a5-11eb-95c4-709d265b03b9.png)](https://user-images.githubusercontent.com/49560745/98510044-bd8cc680-22a5-11eb-95c4-709d265b03b9.png)

### - join.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width" initial-scale="1">
<link rel="stylesheet" href="css/bootstrap.css">
<title>JSP 게시판 웹 사이트</title>
</head>
<body>
	<nav class="navbar navbar-default">
		<div class="navbar-header">
			<button type="button" class="navbar-toggle collapsed" 
				data-toggle="collapse" data-target="#bs-example-navbar-collapse-1"
				aria-expanded="false">
				<span class="icon-bar"></span>
				<span class="icon-bar"></span>
				<span class="icon-bar"></span>
			</button>
			<a class="navbar-brand" href="main.jsp">JSP 게시판 웹사이트 </a>
		</div>
		<div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
			<ul class="nav navbar-nav">
				<li><a href="main.jsp">메인</a></li>
				<li><a href="bbs.jsp">게시판</a></li>
			</ul>
			<ul class="nav navbar-nav navbar-right">
         		<li class="dropdown">
           			<a href="#" class="dropdown-toggle" 
                                data-toggle="dropdown" role="button" aria-haspopup="true" 
            			aria-expanded="false">접속하기 <span class="caret"></span></a>
        			<ul class="dropdown-menu">
              			<li ><a href="login.jsp">로그인</a></li>
              			<li class="active"><a href="join.jsp">회원가입</a></li>
            		</ul>    
         		</li>
       		</ul>
		</div>
	</nav>
	<div class="container">
		<div class="col-lg-4"></div>
		<div class="col-lg-4">
			<div class="jumbtron" style="padding-top: 20px;">
				<form method="post" action="joinAction.jsp">
					<h3 style="text-align: center;">회원가입 화면</h3>
					<div class="form-group">
						<input type="text" class="form-control" placeholder="아이디" name="userID" maxlength="20">
					</div>
					<div class="form-group">
						<input type="password" class="form-control" placeholder="비밀번호" name="userPassword" maxlength="20">
					</div>
					<div class="form-group">
						<input type="text" class="form-control" placeholder="닉네임" name="userName" maxlength="20">
					</div>
					<div class="form-group">
						<input type="email" class="form-control" placeholder="이메일" name="userEmail" maxlength="50">
					</div>

					<div class="form-group">
						<label><input type="checkbox" class="form-control" placeholder="성별" name="userGender" 
						onclick="doOpenCheck(this)" value="남자">남</label>
						<label><input type="checkbox" class="form-control" placeholder="성별" name="userGender" 
						onclick="doOpenCheck(this)" value="여자">여</label>
					</div>
					<input type="submit" class="btn btn-primary form control" value="회원가입">
				</form>
			</div>
		</div>
		<div class="col-lg-4"></div>
	</div>
	<script>
        
			function doOpenCheck(chk){
				let obj=document.getElementsByName("userGender");
				for(let i=0;i<obj.length;i++){
					if(obj[i]!=chk){
						obj[i].checked=false;
					}
				}
			}
			
	</script>
	<script src="https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js"></script>
	<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
</body>
</html>
```

## 3) User 클래스 생성

### - uesr.java

```java
package user;

public class User {
	
	private String userID;
	private String userPassword;
	private String userName;
	private String userGender;
	private String userEmail;
	
	public String getUserID() {
		return userID;
	}
	public void setUserID(String userID) {
		this.userID = userID;
	}
	public String getUserPassword() {
		return userPassword;
	}
	public void setUserPassword(String userPassword) {
		this.userPassword = userPassword;
	}
	public String getUserName() {
		return userName;
	}
	public void setUserName(String userName) {
		this.userName = userName;
	}
	public String getUserGender() {
		return userGender;
	}
	public void setUserGender(String userGender) {
		this.userGender = userGender;
	}
	public String getUserEmail() {
		return userEmail;
	}
	public void setUserEmail(String userEmail) {
		this.userEmail = userEmail;
	}
	
}
```

## 4) UserDAO 클래스 생성

```
1. UserDAO 생성자
객체생성과 동시에 DB에 커넥션을 얻는다.

2. join 메소드
db에 회원 정보를 추가하자.
return -2 // 데이터베이스 오류

* db 오류
String dbURL="jdbc:mysql://localhost:3306/BBS?serverTimezone=Asia/Seoul&useSSL=false";
(? 이후 코드는 오류 발생으로 추가했다.)
```

### - UserDAO.java

```java
package user;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;

public class UserDAO {

	private Connection conn;
	private PreparedStatement pstmt;
	private ResultSet rs;
	
//	Class.forName() 을 이용해서 드라이버 로드
//	DriverManager.getConnection() 으로 연결 얻기
//	Connection 인스턴스를 이용해서 Statement 객체 생성
//	Statement 객체의 결과를 ResultSet 이나 int에 받기
	public UserDAO() {
		try {
			String dbURL="jdbc:mysql://localhost:3306/BBS?serverTimezone=Asia/Seoul&useSSL=false";	
			String dbID="root";
			String dbPassword="root";
			Class.forName("com.mysql.jdbc.Driver"); //드라이버 로드
			conn =DriverManager.getConnection(dbURL,dbID,dbPassword); // 연결 얻기 
			// Type mismatch: cannot convert from java.sql.Connection to com.sun.corba.se.pept.transport.Connection
			// import 확인 하기 
			
		} catch (Exception e) {
			// TODO: handle exception
		}
	}
    
	public int join(User user) {
		String SQL="INSERT INTO USER VALUES (?,?,?,?,?)";
		try {
			pstmt=conn.prepareStatement(SQL);
			pstmt.setString(1,user.getUserID());
			pstmt.setString(2,user.getUserPassword());
			pstmt.setString(3,user.getUserName());
			pstmt.setString(4,user.getUserGender());
			pstmt.setString(5,user.getUserEmail());
			return pstmt.executeUpdate();
		} catch (Exception e) {
			System.out.println("오류가 발생했습니다.");
			// TODO: handle exception
		}
		return -2;
	}
}
```

## 5) 회원가입 액션 처리

```jsp
1. UserDAO / User 클래스를 임포트해주자.

2. 자바빈을 통해 User 객체를 생성하자.
    
----------------------------------------------------------
<jsp:useBean id="user" class="user.User" scope="page"/> 
<%-- 자바에서 객체를 생성하는 것과 동일 
	User user=new User();
--%>
----------------------------------------------------------

3. User 객체에 프로퍼티를 set 하자.
    
----------------------------------------------------------    
<jsp:setProperty name="user" property="userID"/>
<jsp:setProperty name="user" property="userPassword"/>
<jsp:setProperty name="user" property="userName"/>
<jsp:setProperty name="user" property="userGender"/>
<jsp:setProperty name="user" property="userEmail"/>
----------------------------------------------------------
<%-- 폼으로부터 넘어오는 파라미터의 이름과 개수와 setProperty 파라미터의 이름과 개수는 동일해야한다. --%>
 join.jsp의 form 데이터
(name="userID" name="userPassword name="userName" name="userGender" name="userEmail" )
 
4. UserDAO 객체를 생성하고 join을 수행하자.

5. 폼의 비어있는 정보확인하기
-------------------------------------------------------------------------------------
		if(user.getUserID()==null || user.getUserPassword()==null || user.getUserGender()==null ||
				user.getUserEmail()==null || user.getUserName()==null)
-------------------------------------------------------------------------------------
```

### - joinAction.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="user.UserDAO" %>
<%@ page import="java.io.PrintWriter" %>
<% request.setCharacterEncoding("UTF-8"); %>
<jsp:useBean id="user" class="user.User" scope="page"/> 
<%-- 자바에서 객체를 생성하는 것과 동일 
	User user=new User();
--%>
<jsp:setProperty name="user" property="userID"/>
<jsp:setProperty name="user" property="userPassword"/>
<jsp:setProperty name="user" property="userName"/>
<jsp:setProperty name="user" property="userGender"/>
<jsp:setProperty name="user" property="userEmail"/>

<%-- 폼으로부터 넘어오는 파라미터의 이름과 개수와 setProperty 파라미터의 이름과 개수는 동일해야한다. --%>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width" initial-scale="1">
<title>JSP 게시판 웹 사이트</title>
</head>
<body>

	<%
	
		String userID=null;
		if(session.getAttribute("userID")!=null){
			userID=(String)session.getAttribute("userID");
		}
		if(userID!=null){
			PrintWriter script=response.getWriter();
			script.println("<script>");
			script.println("alert('이미 로그인 되어있습니다.')");
			script.println("location.href='main.jsp'");
			script.println("</script>");	
		}
		
		if(user.getUserID()==null || user.getUserPassword()==null || user.getUserGender()==null ||
				user.getUserEmail()==null || user.getUserName()==null){
			PrintWriter script=response.getWriter();
			script.println("<script>");
			script.println("alert('모든 정보를 입력해주세요.')");
			script.println("history.back()");
			script.println("</script>");
		}
		else{
			UserDAO userDAO = new UserDAO();
			int result =userDAO.join(user);
			System.out.println(result);
			if(result==1){
				session.setAttribute("userID", user.getUserID());
				PrintWriter script=response.getWriter();
				script.println("<script>");
				script.println("'회원가입이 완료되었습니다.'");
				script.println("location.href='main.jsp'");
				script.println("</script>");
			}
			else if(result==-2){
				PrintWriter script=response.getWriter();
				script.println("<script>");
				script.println("alert('데이터베이스 오류가 발생했습니다.')");
				script.println("history.back()"); // 이전 상황으로 되돌리기
				script.println("</script>");		
			}
		}
	%>

</body>
</html>
```

# Reference

- https://www.youtube.com/playlist?list=PLRx0vPvlEmdAZv_okJzox5wj2gG_fNh_6