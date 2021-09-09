---
title: JSP 게시판 제작[4] - 로그인
toc: true
categories:	
    - JSP Board
tags: 
- Java
- JSP
last_modified_at:
---

```
1) 로그인 폼 만들기
2) 로그인 기능 동작 페이지 만들기
3) 로그아웃 기능 동작 페이지 만들기

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
- login.jsp
- loginAction.jsp
- logoutAction.jsp
-------------------------
```

## 1) 로그인 화면 (폼) 만들기

```
1) 회원가입 페이지와 동일하게 폼 형식으로 테이블의 프로퍼티를 전달하자.
```

[![image](https://user-images.githubusercontent.com/49560745/98513962-15c6c700-22ac-11eb-8b0a-04a4bd85bb42.png)](https://user-images.githubusercontent.com/49560745/98513962-15c6c700-22ac-11eb-8b0a-04a4bd85bb42.png)

### - login.jsp

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
              			<li class="active"><a href="login.jsp">로그인</a></li>
              			<li><a href="join.jsp">회원가입</a></li>
            		</ul>    
         		</li>
       		</ul>
		</div>
	</nav>
	<div class="container">
		<div class="col-lg-4"></div>
		<div class="col-lg-4">
			<div class="jumbtron" style="padding-top: 20px;">
				<form method="post" action="loginAction.jsp">
					<h3 style="text-align: center;">로그인 화면</h3>
					<div class="form-group">
						<input type="text" class="form-control" placeholder="아이디" name="userID" maxlength="20">
					</div>
					<div class="form-group">
						<input type="password" class="form-control" placeholder="비밀번호" name="userPassword" maxlength="20">
					</div>
					<input type="submit" class="btn btn-primary form control" value="로그인">
				</form>
			</div>
		</div>
		<div class="col-lg-4"></div>
	</div>
	<script src="https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js"></script>
	<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
</body>
</html>
```

## 2) UserDAO 클래스 login 메소드

```
- join은 updateQuery() 라면
login은 executeQuert() 임을 기억하자.

- login 메소드
1. 아이디가 존재확인(else면 return -1)
2. 패스워드 일치 확인(일치하면 return 1 / else면 return 0)
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
	
	public int login(String userID, String userPassword) {
		String SQL="SELECT userPassword FROM USER WHERE userID=?";
		try {
			pstmt=conn.prepareStatement(SQL); // SQL 실행 객체
			pstmt.setString(1, userID);  // SQL 객체의 첫 번째 물음표 값 지정
			rs=pstmt.executeQuery();
			if(rs.next()) {
				if(rs.getString(1).equals(userPassword)) {
					return 1;
				}else {
					return 0;
				}
			}
			return -1;
		} catch (Exception e) {
			// TODO: handle exception
		}
		return -2;
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

## 3) 로그인 액션 처리

```
1) 마찬가지로 자바빈을 활용해 객체를 생성하고, 프로퍼티를 set 한다.
------------------------------------------------------------
<% request.setCharacterEncoding("UTF-8"); %>
<jsp:useBean id="user" class="user.User" scope="page"/>
<jsp:setProperty name="user" property="userID"/>
<jsp:setProperty name="user" property="userPassword"/>
------------------------------------------------------------

구현은 회원가입과 동일하다.
```

[![image](https://user-images.githubusercontent.com/49560745/98514149-62aa9d80-22ac-11eb-9144-62bccd5820f6.png)](https://user-images.githubusercontent.com/49560745/98514149-62aa9d80-22ac-11eb-9144-62bccd5820f6.png)

### - loginAction.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="user.UserDAO" %>
<%@ page import="java.io.PrintWriter" %>
<% request.setCharacterEncoding("UTF-8"); %>
<jsp:useBean id="user" class="user.User" scope="page"/>
<jsp:setProperty name="user" property="userID"/>
<jsp:setProperty name="user" property="userPassword"/>
<!--property의 값을 login.jsp의 값과 동일하게 써주면 login.jsp 페이지의 데이터를 가져온다.-->
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
	
		UserDAO userDAO = new UserDAO();
		int result =userDAO.login(user.getUserID(),user.getUserPassword());
		if(result==1){
			session.setAttribute("userID", user.getUserID());
			PrintWriter script=response.getWriter();
			script.println("<script>");
			script.println("location.href='main.jsp'");
			script.println("</script>");
		}
		else if(result==0){
			PrintWriter script=response.getWriter();
			script.println("<script>");
			script.println("alert('비밀번호가 틀립니다.')");
			script.println("history.back()");
			script.println("</script>");		
		}
		else if(result==-1){
			PrintWriter script=response.getWriter();
			script.println("<script>");
			script.println("alert('존재하지 않는 아이디입니다.')");
			script.println("history.back()");
			script.println("</script>");		
		}
		else if(result==-2){
			PrintWriter script=response.getWriter();
			script.println("<script>");
			script.println("alert('데이터베이스 오류가 발생했습니다.')");
			script.println("history.back()");
			script.println("</script>");		
		}
	%>

</body>
</html>
```

## 4) 로그아웃 액션 처리

```
- 세션 invalidate 메소드로 세션연결 끊기
```

### - logoutAction.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>

	<%
		session.invalidate();
	%>
	
	<script>
		location.href='main.jsp';
	</script>
</body>
</html>
```

# Reference

- https://www.youtube.com/playlist?list=PLRx0vPvlEmdAZv_okJzox5wj2gG_fNh_6