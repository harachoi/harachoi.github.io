---
title: JSP 게시판 제작[7] - 게시판 글 목록
toc: true
categories:	
    - JSP Board
tags: 
- Java
- JSP
last_modified_at:
---

```
1) BbsDAO에 nextPage() / getList() / getNext() 메소드 추가

nextPage() : 다음 페이지
getList() : 게시판 목록

2) 게시판 글 목록 로드


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
- writer.jsp
- writerAction.jsp
- bbs.jsp
-------------------------
```

## 1) BbsDAO 리스트, 쓰기 기능 추가

```
1) getList() => 글 목록 리스트 가져오기

String SQL="SELECT * FROM BBS WHERE bbsID<? AND bbsAvailable=1 ORDER BY bbsID DESC LIMIT 10";

아래 그림 참고

2) getNext() => 현재 게시글의 개수 +1 반환

쿼리를 수행할 때 마다 list에 담아주자.

3) nextPage() => 페이지 설정(1page 당 글 10개)
```

[![image](https://user-images.githubusercontent.com/49560745/98535903-3aca3280-22ca-11eb-9d2b-b0738a7b56bf.png)](https://user-images.githubusercontent.com/49560745/98535903-3aca3280-22ca-11eb-9d2b-b0738a7b56bf.png)

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
	
	public int getNext() {
		String SQL="select bbsID FROM BBS ORDER BY bbsID DESC";
		try {
		
			PreparedStatement pstmt=conn.prepareStatement(SQL);
			rs=pstmt.executeQuery();
			if(rs.next()) {
				System.out.println(rs.getInt(1)); // select문에서 첫번째 값
				return rs.getInt(1)+1;  // 현재 인덱스(현재 게시글 개수) +1 반환
			}
			return 1;
		} catch (Exception e) {
			e.printStackTrace();
		}
		return -1;
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
	
	public ArrayList<Bbs> getList(int pageNumber){
		String SQL="SELECT * FROM BBS WHERE bbsID<? AND bbsAvailable=1 ORDER BY bbsID DESC LIMIT 10";
		ArrayList<Bbs> list =new ArrayList<Bbs>();
		try {
			PreparedStatement pstmt=conn.prepareStatement(SQL);
			pstmt.setInt(1,getNext()-(pageNumber-1)*10);
			rs=pstmt.executeQuery();
			while(rs.next()) {
				Bbs bbs=new Bbs();
				bbs.setBbsID(rs.getInt(1));
				bbs.setBbsTitle(rs.getString(2));
				bbs.setUserID(rs.getString(3));
				bbs.setBbsDate(rs.getString(4));
				bbs.setBbsContent(rs.getString(5));
				bbs.setBbsAvailable(rs.getInt(6));
				list.add(bbs);
			}
		} catch (Exception e) {
			e.printStackTrace();
		}
		return list;
	}
	
	
	public boolean nextPage(int pageNumber) {
		String SQL="SELECT * FROM BBS WHERE bbsID<? AND bbsAvailable=1 ORDER BY bbsID DESC LIMIT 10";
		try {
			PreparedStatement pstmt=conn.prepareStatement(SQL);
			pstmt.setInt(1,getNext()-(pageNumber-1)*10);
			rs=pstmt.executeQuery();
			if(rs.next()) {
				return true;
			}
		} catch (Exception e) {
			e.printStackTrace();
		}
		return false;
	}
	

}
```

## 2) 게시판 글 목록 로드

```
1) autoincrement 설정
글 번호 자동 증가
2) 글 목록을 10개 단위로 로드

pstmt.setInt(1,getNext()-(pageNumber-1)*10);
=> (현재 글 개수 +1 )  - (현재페이지넘버 -1) * 10
```

[![image](https://user-images.githubusercontent.com/49560745/98535043-ccd13b80-22c8-11eb-82a4-b941f59d247b.png)](https://user-images.githubusercontent.com/49560745/98535043-ccd13b80-22c8-11eb-82a4-b941f59d247b.png)

### - bbs.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.io.PrintWriter" %>
<%@ page import="bbs.BbsDAO" %>
<%@ page import="bbs.Bbs" %>
<%@ page import="java.util.ArrayList" %>
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
		int pageNumber=1;
		// pageNumber는 URL에서 가져온다.
		if(request.getParameter("pageNumber")!=null){
			pageNumber=Integer.parseInt(request.getParameter("pageNumber"));
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
					<%
						BbsDAO bbsDAO=new BbsDAO();
						ArrayList<Bbs> list= bbsDAO.getList(pageNumber);
						for(int i=list.size()-1;i>=0;i--){
					%>
					<tr>
						<td><%= list.size()-i %></td>
						<td><a href="view.jsp?bbsID=<%= list.get(i).getBbsID() %>"><%= list.get(i).getBbsTitle().replaceAll(" ", "&nbsp;").replaceAll("<","&lt;").replaceAll(">","&gt;").replaceAll("\n","<br>;")  %></a></td>
 						<td><%= list.get(i).getUserID() %></td>
						<td><%= list.get(i).getBbsDate().substring(0,11)+list.get(i).getBbsDate().substring(11,13)+"시"+ list.get(i).getBbsDate().substring(14,16)+"분" %></td>
					</tr>
					<%
						}
					%>
				</tbody>
			</table>	
			<%
				if(pageNumber!=1){
				
			%>
				<a href="bbs.jsp?pageNumber=<%=pageNumber -1 %>" class="btn btn-success btn-arraw-left">이전</a>
			<%
			
				} if(bbsDAO.nextPage(pageNumber+1)){
			%>
				<a href="bbs.jsp?pageNumber=<%=pageNumber +1 %>" class="btn btn-success btn-arraw-left">다음</a>
			
			<%
				}
			%>
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