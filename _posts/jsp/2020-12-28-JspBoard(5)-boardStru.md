---
title: JSP 게시판 제작[5] - 게시판 구조
toc: true
categories:	
    - JSP Board
tags: 
- Java
- JSP
last_modified_at:
---

```
1) 게시판 테이블 생성
2) Bbs 클래스 생성
3) BbsDAO 클래스 생성
4) 게시판 화면 구현


디렉토리 구조
-------------------------
src
- bbs
	- Bbs.java
	- BbsDAO.java
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
- bbs.jsp
-------------------------
```

## 1) 게시판 테이블 생성

```
- bbsID 를 PRIMARY KEY로 설정하자.
- bbsAvailable
게시판 삭제 여부 판단 위함
게시판 삭제 기능 수행시 바로 데이터베이스에서 삭제되지 않도록 한다.
=> 나중에 log를 추적하거나 문제 발생시 활용하기 위함.

- SQL 문

create table bbs(
	bbsID int PRIMARY KEY,
	bbsTitle VARCHAR(50),
	userID VARCAHR(20),
	bbsDATE VARCHAR(20),
	bbsContent VARCHAR(2048),
	bbsAvailable int
);
```

[![image](https://user-images.githubusercontent.com/49560745/98515141-f466da80-22ad-11eb-9ceb-b184fe57d8a0.png)](https://user-images.githubusercontent.com/49560745/98515141-f466da80-22ad-11eb-9ceb-b184fe57d8a0.png)

## 2) 게시판 화면 만들기

```
- 게시판 클래스 생성
```

### - Bbs.java

```java
package bbs;

public class Bbs {
	
	private int bbsID;
	private String bbsTitle;
	private String userID;
	private String bbsDate;
	private String bbsContent;
	private int bbsAvailable;
	public int getBbsID() {
		return bbsID;
	}
	public void setBbsID(int bbsID) {
		this.bbsID = bbsID;
	}
	public String getBbsTitle() {
		return bbsTitle;
	}
	public void setBbsTitle(String bbsTitle) {
		this.bbsTitle = bbsTitle;
	}
	public String getUserID() {
		return userID;
	}
	public void setUserID(String userID) {
		this.userID = userID;
	}
	public String getBbsDate() {
		return bbsDate;
	}
	public void setBbsDate(String bbsDate) {
		this.bbsDate = bbsDate;
	}
	public String getBbsContent() {
		return bbsContent;
	}
	public void setBbsContent(String bbsContent) {
		this.bbsContent = bbsContent;
	}
	public int getBbsAvailable() {
		return bbsAvailable;
	}
	public void setBbsAvailable(int bbsAvailable) {
		this.bbsAvailable = bbsAvailable;
	}
}
```

## 3) 게시판 DAO 만들기

```
- 생성자로 DB 커넥션 얻기
```

### - bbsDAO.java

```java
package bbs;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.ArrayList;

public class BbsDAO {
	private Connection conn;
	private ResultSet rs;
	
	public BbsDAO() {
		try {
			String dbURL="jdbc:mysql://localhost:3306/BBS?serverTimezone=Asia/Seoul&useSSL=false";	
			String dbID="root";
			String dbPassword="root";
			Class.forName("com.mysql.jdbc.Driver"); //드라이버 로드
			conn =DriverManager.getConnection(dbURL,dbID,dbPassword); // 연결 얻기 

		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```

## 4) 게시판 화면 구현

```
1) 게시판 테이블 구조를 만들자

tbody 부분은 게시판 글을 load하는 과정에서 구현하자.
```

[![image](https://user-images.githubusercontent.com/49560745/98533708-a8745f80-22c6-11eb-93f9-85a481c11214.png)](https://user-images.githubusercontent.com/49560745/98533708-a8745f80-22c6-11eb-93f9-85a481c11214.png)

### - bbs.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.io.PrintWriter" %>
<%@ page import="bbs.BbsDAO" %>
<%@ page import="bbs.Bbs" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width" initial-scale="1">
<link rel="stylesheet" href="css/bootstrap.css">
<title>JSP 게시판 웹 사이트</title>
</head>
<body>
	<% 
		String userID=null;
		if(session.getAttribute("userID")!=null){
			userID=(String)session.getAttribute("userID");
		}

	%>

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
		
		<%-- 네비게이션 바 login 되어있음 => 로그아웃 login 안되어있음 => 회원가입, 로그인
			
			class=active를 포함하면 li 태그에 표식 생김
		
		 --%>
		<%
			if(userID==null){
		
		%>
		<div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
			<ul class="nav navbar-nav">
				<li><a href="main.jsp">메인</a></li>
				<li class="active"><a href="bbs.jsp">게시판</a></li>
			</ul>
			<ul class="nav navbar-nav navbar-right">
         		<li class="dropdown">
           			<a href="#" class="dropdown-toggle" 
                                data-toggle="dropdown" role="button" aria-haspopup="true" 
            			aria-expanded="false">접속하기 <span class="caret"></span></a>
        			<ul class="dropdown-menu">
              			<li><a href="login.jsp">로그인</a></li>
              			<li class="active"><a href="join.jsp">회원가입</a></li>
            		</ul>    
         		</li>
       		</ul>
		</div>

		<%
			}else{
		%>
			<div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
			<ul class="nav navbar-nav">
				<li><a href="main.jsp">메인</a></li>
				<li class="active"><a href="bbs.jsp">게시판</a></li>
			</ul>
			<ul class="nav navbar-nav navbar-right">
         		<li class="dropdown">
           			<a href="#" class="dropdown-toggle" 
                                data-toggle="dropdown" role="button" aria-haspopup="true" 
            			aria-expanded="false">접속하기 <span class="caret"></span></a>
        			<ul class="dropdown-menu">
              			<li><a href="logoutAction.jsp">로그아웃</a></li>
            		</ul>    
         		</li>
       		</ul>
		</div>
		<%
			}
		%>
		
	</nav>
	<div class="container">
		<div class="row">
			<table class="table table-striped" style="text-align:center; border :1px solid #dddddd" > <%-- 홀,짝 행 구분 --%>
				<thead>
					<tr>
						<th style="background-color : #eeeeeee; text-align:center;">번호</th>
						<th style="background-color : #eeeeeee; text-align:center;">제목</th>
						<th style="background-color : #eeeeeee; text-align:center;">작성자</th>
						<th style="background-color : #eeeeeee; text-align:center;">작성일</th>
					</tr>
				</thead>
				<tbody>

				</tbody>
			</table>	

			<a href='writer.jsp' class="btn btn-primary pull-right">글쓰기</a>
		</div>
	</div>

	<script src="https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js"></script>
	<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
</body>
</html>
```

# Reference

- https://www.youtube.com/playlist?list=PLRx0vPvlEmdAZv_okJzox5wj2gG_fNh_6