---
title: JSP 게시판 제작[6] - 게시판 글쓰기
toc: true
categories:	
    - JSP Board
tags: 
- Java
- JSP
last_modified_at:
---

```
1) BbsDAO에 getDate() / write() 메소드 추가

getDate() : 현재 날짜
write() : 게시판 글쓰기

2) 게시판 글쓰기 페이지 생성
3) 게시판 글쓰기 동작 페이지 만들기


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
- write.jsp
- writeAction.jsp
- bbs.jsp
-------------------------
```

## 1) DAO 날짜 / 쓰기 메소드 추가

```
1) getDate() 메소드 현재 날짜/시간 return
2) write() 메소드 글 쓰기 기능 구현
```

### - BbsDAO.java

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

	public String getDate() {
		String SQL="select now()";
		try {
		
			PreparedStatement pstmt=conn.prepareStatement(SQL);
			rs=pstmt.executeQuery();
			if(rs.next()) {
				return rs.getString(1);  // 현재 날짜 반환
			}
		} catch (Exception e) {
			e.printStackTrace();
		}
		return "";
	}

	
	public int write(String bbsTitle,String userID, String bbsContent) {
		String SQL="INSERT INTO BBS VALUES(?,?,?,?,?,?)";
		try {
			
			// 1) 쿼리문장분석 2) 컴파일 3) 실행
			// vs Statement
			// 캐시 여부 => PreparedStatement 1)~3) 최초한번 실행 하고 캐시에 저장
			// Statement 매번 1)~3) 실행
			
			PreparedStatement pstmt=conn.prepareStatement(SQL);
			pstmt.setInt(1,getNext());
			pstmt.setString(2,bbsTitle);
			pstmt.setString(3,userID);
			pstmt.setString(4,getDate());
			pstmt.setString(5,bbsContent);
			pstmt.setInt(6,1);
			return pstmt.executeUpdate();
		} catch (Exception e) {
			e.printStackTrace();
		}
		return -1;
	}


}
```

## 2) 게시판 글쓰기 페이지 구현

```
1) 마찬가지로 form 형식로 테이블 프로퍼티를 전달해주자.
```

[![image](https://user-images.githubusercontent.com/49560745/98516195-891e0800-22af-11eb-865a-5136b9783c44.png)](https://user-images.githubusercontent.com/49560745/98516195-891e0800-22af-11eb-865a-5136b9783c44.png)

### - write.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.io.PrintWriter" %>
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
				<li class="active"><a href="main.jsp">메인</a></li>
				<li><a href="bbs.jsp">게시판</a></li>
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
		<form method="post" action="writerAction.jsp">
			<table class="table table-striped" style="text-align:center; border :1px solid #dddddd" > <%-- 홀,짝 행 구분 --%>
				<thead>
					<tr>
						<th colspan="2" style="background-color : #eeeeeee; text-align:center;">게시판 글쓰기 양식</th>
					</tr>
				</thead>
				<tbody>
					<tr>
						<td><input type="text" class="form-control" placeholder="글 제목" name="bbsTitle" maxlength="50"></td>
					</tr>
					<tr>
						<td><textarea type="text" class="form-control" placeholder="글 내용" name="bbsContent" maxlength="2048"></textarea></td>
					</tr>
				</tbody>
			</table>	
			<input type="submit" class="btn btn-primary pull-right" value="글쓰기">
		</form>
		</div>
	</div>

	<script src="https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js"></script>
	<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
</body>
</html>
```

### - writeAction.jsp

```jsp
- loginAction / joinAction 구현과 동일하다.
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="bbs.BbsDAO" %>
<%@ page import="java.io.PrintWriter" %>
<% request.setCharacterEncoding("UTF-8"); %>
<jsp:useBean id="bbs" class="bbs.Bbs" scope="page"/>
<jsp:setProperty name="bbs" property="bbsTitle"/>
<jsp:setProperty name="bbs" property="bbsContent"/>
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
		if(userID==null){
			PrintWriter script=response.getWriter();
			script.println("<script>");
			script.println("alert('로그인이 필요합니다.')");
			script.println("location.href='login.jsp'");
			script.println("</script>");	
		}
		else{
			if(bbs.getBbsTitle()==null || bbs.getBbsContent()==null){
				PrintWriter script= response.getWriter();
				script.println("<script>");
				script.println("alert('제목,게시글 내용이 비어있습니다.')");
				script.println("history.back()");
				script.println("</script>");
			}
			else{
				BbsDAO bbsDAO=new BbsDAO();
				int result = bbsDAO.write(bbs.getBbsTitle(),userID,bbs.getBbsContent());
				if(result==-1){
					PrintWriter script= response.getWriter();
					script.println("<script>");
					script.println("alert('글쓰기에 실패했습니다.')");
					script.println("history.back()");
					script.println("</script>");
				}
				else{
					PrintWriter script= response.getWriter();
					script.println("<script>");
					script.println("location.href='bbs.jsp'");
					script.println("</script>");
				}
			}
		}
	%>
</body>
</html>
```

# Reference

- https://www.youtube.com/playlist?list=PLRx0vPvlEmdAZv_okJzox5wj2gG_fNh_6