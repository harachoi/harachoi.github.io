---
title: JSP 게시판 제작[8] - 글 조회/삭제/댓글/수정
toc: true
categories:	
    - JSP Board
tags: 
- Java
- JSP
last_modified_at:
---

```
1) 게시판 아이디(번호)에 해당하는 글 조회
2) 게시판 삭제 기능
3) 댓글 기능
4) 수정 기능

디렉토리 구조
-------------------------
src
- reply
	- Reply.java
	- ReplyDAO.java
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
- writeAction.jsp
- bbs.jsp
- view.jsp
- deleteAction.jsp
- replyAction.jsp
- update.jsp
- updateAction.jsp
-------------------------
```

[![image](https://user-images.githubusercontent.com/49560745/98537750-158af380-22cd-11eb-962f-3804a91b6dcc.png)](https://user-images.githubusercontent.com/49560745/98537750-158af380-22cd-11eb-962f-3804a91b6dcc.png)

## 1) 글 조회 기능

```
1) 해당 글 번호에 맞게 URL 설정하기
<form method="post" action="replyAction.jsp?bbsID=<%= bbsID %>">
    
1. bbsID에 해당하는 글 내용을 데이터베이스에서 가져온다. 
2. bbsID 고유의 게시글 번호로 url을 만들고, 1의 내용을 가져와 뿌려준다.
```

### - view.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.io.PrintWriter" %>
<%@ page import="bbs.Bbs" %>
<%@ page import="bbs.BbsDAO" %>
<%@ page import="reply.Reply" %>
<%@ page import="reply.ReplyDAO" %>
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
		
    
    	
		int bbsID=0;
		// url의 bbsID를 가져온다.
    	if(request.getParameter("bbsID")!=null){
			bbsID=Integer.parseInt(request.getParameter("bbsID"));
		}
		
		int pageNumber=1;
		// pageNumber는 URL에서 가져온다.
		if(request.getParameter("pageNumber")!=null){
			pageNumber=Integer.parseInt(request.getParameter("pageNumber"));
		}
		
		if(bbsID==0){
			PrintWriter script=response.getWriter();
			script.println("<script>");
			script.println("alert('유효하지 않은 글입니다.')");
			script.println("location.href='bbs.jsp'");
			script.println("</script>");	
		}
		Bbs bbs=new BbsDAO().getBbs(bbsID);
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
			<table class="table table-striped" style="text-align:center; border :1px solid #dddddd" > <%-- 홀,짝 행 구분 --%>
				<thead>
					<tr>
						<th colspan="3" style="background-color : #eeeeeee; text-align:center;">게시판 글 보기</th>
					</tr>
				</thead>
				<tbody>
					<tr>
						<td style="width:20%;">글 제목 </td>	
						<td colspan="2"><%= bbs.getBbsTitle() %></td>
					</tr>
					<tr>
						<td >작성자 </td>	
						<td colspan="2"><%= bbs.getUserID() %></td>
					</tr>
					<tr>
						<td >작성일자</td>	
						<td colspan="2"><%= bbs.getBbsDate().substring(0,11)+bbs.getBbsDate().substring(11,13)+"시"+bbs.getBbsDate().substring(14,16)+"분" %></td>
					</tr>
					<tr>
						<td >내용</td>	
						<td colspan="2" style="min-height:200px; text-align:left;"><%= bbs.getBbsContent().replaceAll(" ", "&nbsp;").replaceAll("<","&lt;").replaceAll(">","&gt;").replaceAll("\n","<br>;") %></td>
					</tr>
					
					
				</tbody>
			
			</table>
			<form method="post" action="replyAction.jsp?bbsID=<%= bbsID %>">
				<table class="table table-striped"
					style="text-align: center; border: 1px solid #dddddd">
					<%-- 홀,짝 행 구분 --%>
					<thead>
						<tr>
							<th colspan="3"
								style="background-color: #eeeeeee; text-align: center;">댓글</th>
						</tr>
					</thead>
					<tbody>
					
						<%
							ReplyDAO replyDAO=new ReplyDAO();
							ArrayList<Reply> list=replyDAO.getList(bbsID, pageNumber);
							for(int i=list.size()-1;i>=0;i--){
							
						%>

						<tr>
							<td style="text-align: left;"><%= list.get(i).getReplyContent() %></td>
							<td style="text-align: right;"><%= list.get(i).getUserID() %>
							<a href="update.jsp?bbsID=<%= bbsID %>" class="btn">수정</a>
							<a href="update.jsp?bbsID=<%= bbsID %>" class="btn ">삭제</a>
							</td>
						</tr>
					
						<%
								}
						%>
						<td><textarea type="text" class="form-control"
								placeholder="댓글을 입력하세요." name="replyContent" maxlength="2048"></textarea></td>
						<td style="text-align: left; "></td>
					
					</tbody>
				</table>
				<input type="submit" class="btn" value="댓글입력">
			</form>
			<br>
			<a href="bbs.jsp" class="btn btn-primary">목록</a>
			
			<%
				if(userID!=null && userID.equals(bbs.getUserID())){
			%>
				<a href="update.jsp?bbsID=<%= bbsID %>" class="btn btn-primary">수정</a>
				<a href="deleteAction.jsp?bbsID=<%= bbsID %>" class="btn btn-primary">삭제</a>
			
			<%
			} 
			%>
			
		</div>
	</div>

	<script src="https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js"></script>
	<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
</body>
</html>
```

## 2) 글 삭제 기능

```
1) delete() 메소드 추가
2) deleteAction.jsp 추가
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
	
	public int delete(int BbsID) {
		String SQL="UPDATE BBS SET bbsAvailable=0 WHERE BbsID=?";
		try {
			PreparedStatement pstmt= conn.prepareStatement(SQL);
			pstmt.setInt(1,BbsID);
			pstmt.executeUpdate();
			return 1;
		} catch (Exception e) {
			e.printStackTrace();
		}
		return -1;
	}
	
	public int update(int bbsID,String bbsTitle,String bbsContent) {
		String SQL="UPDATE BBS SET bbsTitle=?, bbsContent=? WHERE bbsID=?";
		try {
			PreparedStatement pstmt= conn.prepareStatement(SQL);
			pstmt.setString(1, bbsTitle);
			pstmt.setString(2, bbsContent);
			pstmt.setInt(3,bbsID);
			pstmt.executeUpdate();
			return 1;
		} catch (Exception e) {
			e.printStackTrace();
		}
		
		return -1;
	}
	
	public Bbs getBbs(int bbsID) {
		String SQL="SELECT * FROM BBS WHERE bbsID=?";
		try {
			PreparedStatement pstmt=conn.prepareStatement(SQL);
			pstmt.setInt(1,bbsID);
			rs=pstmt.executeQuery();
			if(rs.next()) {
				Bbs bbs=new Bbs();
				bbs.setBbsID(rs.getInt(1));
				bbs.setBbsTitle(rs.getString(2));
				bbs.setUserID(rs.getString(3));
				bbs.setBbsDate(rs.getString(4));
				bbs.setBbsContent(rs.getString(5));
				bbs.setBbsAvailable(rs.getInt(6));
				return bbs;
			}
			
		} catch (Exception e) {
			e.printStackTrace();
		}
		return null;
	}
}
```

### - deleteAction.jsp

```jsp
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
		int bbsID=1;
		if(request.getParameter("bbsID")!=null){
			bbsID=Integer.parseInt(request.getParameter("bbsID"));
		}
		System.out.println("delete + "+ bbsID);
		if(userID==null){
			PrintWriter script=response.getWriter();
			script.println("<script>");
			script.println("alert('로그인이 필요합니다.')");
			script.println("location.href='login.jsp'");
			script.println("</script>");	
		}
		else{
				BbsDAO bbsDAO=new BbsDAO();
				int result = bbsDAO.delete(bbsID);
				if(result==-1){
					PrintWriter script= response.getWriter();
					script.println("<script>");
					script.println("alert('글 삭제에 실패했습니다.')");
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
	%>
</body>
</html>
```

## 3) 글 댓글 기능

```
1) reply 테이블 구축

reply 테이블의 bbsID => bbs 테이블의 bbsID 
외래키 => 기본키
연결

"게시판 번호에 해당하는 댓글을 로드하자."
-------------------------------
create table reply(
	userID varchar(20),
	replyID int primary key,
	replyContent varchar(20),
	bbsID int foreign key,
	replyAvailable int
);
-------------------------------
```

[![image](https://user-images.githubusercontent.com/49560745/98538054-892d0080-22cd-11eb-9d0c-832fc9a3c8d6.png)](https://user-images.githubusercontent.com/49560745/98538054-892d0080-22cd-11eb-9d0c-832fc9a3c8d6.png)

```
2) reply 클래스 구현
```

### - Reply.java

```java
package reply;

public class Reply {
	
	private int bbsID;
	private int replyID;
	private String replyContent;
	private String userID;
	private int replyAvailable;
	
	public int getBbsID() {
		return bbsID;
	}
	public void setBbsID(int bbsID) {
		this.bbsID = bbsID;
	}
	public int getReplyID() {
		return replyID;
	}
	public void setReplyID(int replyID) {
		this.replyID = replyID;
	}
	public String getReplyContent() {
		return replyContent;
	}
	public void setReplyContent(String replyContent) {
		this.replyContent = replyContent;
	}
	public String getUserID() {
		return userID;
	}
	public void setUserID(String userID) {
		this.userID = userID;
	}
	public int getReplyAvailable() {
		return replyAvailable;
	}
	public void setReplyAvailable(int replyAvailable) {
		this.replyAvailable = replyAvailable;
	}
}
3) replyDAO 클래스 구현
```

### - ReplyDAO.java

```java
package reply;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.ArrayList;

public class ReplyDAO {

	private Connection conn;
	private ResultSet rs;
	
	public ReplyDAO() {
		try {
			String dbURL="jdbc:mysql://localhost:3306/BBS?serverTimezone=Asia/Seoul&useSSL=false";	
			String dbID="root";
			String dbPassword="root";
			Class.forName("com.mysql.jdbc.Driver");
			conn=DriverManager.getConnection(dbURL,dbID,dbPassword);
		
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
	
	
	public ArrayList<Reply> getList(int bbsID,int pageNumber){
		String SQL="SELECT * FROM REPLY WHERE replyID<? AND replyAvailable=1 AND bbsID=? ORDER BY replyID DESC LIMIT 10";
		ArrayList<Reply> list=new ArrayList<Reply>();
		try {
			PreparedStatement pstmt=conn.prepareStatement(SQL);
			pstmt.setInt(1,getNext()-(pageNumber-1)*10);
			pstmt.setInt(2, bbsID);
			rs=pstmt.executeQuery();
			while(rs.next()) {
				Reply reply=new Reply();
				reply.setUserID(rs.getString(1));
				reply.setReplyID(rs.getInt(2));
				reply.setReplyContent(rs.getString(3));
				reply.setBbsID(bbsID);
				reply.setReplyAvailable(1); // rs.getInt(5) => out of index 오류
				list.add(reply);
			}
		} catch (Exception e) {
			e.printStackTrace();
		}
		return list;
	}
	
	public int getNext() {
		String SQL="select replyID FROM REPLY ORDER BY replyID DESC";
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
	public int write(int bbsID,String replyContent,String userID) {
		String SQL="INSERT INTO REPLY VALUES(?,?,?,?,?)";
		
		try {
			PreparedStatement pstmt=conn.prepareStatement(SQL);
			pstmt.setString(1,userID);
			pstmt.setInt(2, getNext());
			pstmt.setString(3, replyContent);
			pstmt.setInt(4,bbsID);
			pstmt.setInt(5,1);
			return pstmt.executeUpdate();
		} catch (Exception e) {
			e.printStackTrace();
		}
		return -1;
	}
	
}
4) view.jsp에 댓글 기능 추가
```

### - view.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.io.PrintWriter" %>
<%@ page import="bbs.Bbs" %>
<%@ page import="bbs.BbsDAO" %>
<%@ page import="reply.Reply" %>
<%@ page import="reply.ReplyDAO" %>
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
		
		int bbsID=0;
		if(request.getParameter("bbsID")!=null){
			bbsID=Integer.parseInt(request.getParameter("bbsID"));
		}
		
		int pageNumber=1;
		// pageNumber는 URL에서 가져온다.
		if(request.getParameter("pageNumber")!=null){
			pageNumber=Integer.parseInt(request.getParameter("pageNumber"));
		}
		
		if(bbsID==0){
			PrintWriter script=response.getWriter();
			script.println("<script>");
			script.println("alert('유효하지 않은 글입니다.')");
			script.println("location.href='bbs.jsp'");
			script.println("</script>");	
		}
		Bbs bbs=new BbsDAO().getBbs(bbsID);
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
			<table class="table table-striped" style="text-align:center; border :1px solid #dddddd" > <%-- 홀,짝 행 구분 --%>
				<thead>
					<tr>
						<th colspan="3" style="background-color : #eeeeeee; text-align:center;">게시판 글 보기</th>
					</tr>
				</thead>
				<tbody>
					<tr>
						<td style="width:20%;">글 제목 </td>	
						<td colspan="2"><%= bbs.getBbsTitle() %></td>
					</tr>
					<tr>
						<td >작성자 </td>	
						<td colspan="2"><%= bbs.getUserID() %></td>
					</tr>
					<tr>
						<td >작성일자</td>	
						<td colspan="2"><%= bbs.getBbsDate().substring(0,11)+bbs.getBbsDate().substring(11,13)+"시"+bbs.getBbsDate().substring(14,16)+"분" %></td>
					</tr>
					<tr>
						<td >내용</td>	
						<td colspan="2" style="min-height:200px; text-align:left;"><%= bbs.getBbsContent().replaceAll(" ", "&nbsp;").replaceAll("<","&lt;").replaceAll(">","&gt;").replaceAll("\n","<br>;") %></td>
					</tr>
					
					
				</tbody>
			
			</table>
			<form method="post" action="replyAction.jsp?bbsID=<%= bbsID %>">
				<table class="table table-striped"
					style="text-align: center; border: 1px solid #dddddd">
					<%-- 홀,짝 행 구분 --%>
					<thead>
						<tr>
							<th colspan="3"
								style="background-color: #eeeeeee; text-align: center;">댓글</th>
						</tr>
					</thead>
					<tbody>
					
						<%
							ReplyDAO replyDAO=new ReplyDAO();
							ArrayList<Reply> list=replyDAO.getList(bbsID, pageNumber);
							for(int i=list.size()-1;i>=0;i--){
							
						%>

						<tr>
							<td style="text-align: left;"><%= list.get(i).getReplyContent() %></td>
							<td style="text-align: right;"><%= list.get(i).getUserID() %>
							<a href="update.jsp?bbsID=<%= bbsID %>" class="btn">수정</a>
							<a href="update.jsp?bbsID=<%= bbsID %>" class="btn ">삭제</a>
							</td>
						</tr>
					
						<%
								}
						%>
						<td><textarea type="text" class="form-control"
								placeholder="댓글을 입력하세요." name="replyContent" maxlength="2048"></textarea></td>
						<td style="text-align: left; "></td>
					
					</tbody>
				</table>
				<input type="submit" class="btn" value="댓글입력">
			</form>
			<br>
			<a href="bbs.jsp" class="btn btn-primary">목록</a>
			
			<%
				if(userID!=null && userID.equals(bbs.getUserID())){
			%>
				<a href="update.jsp?bbsID=<%= bbsID %>" class="btn btn-primary">수정</a>
				<a href="deleteAction.jsp?bbsID=<%= bbsID %>" class="btn btn-primary">삭제</a>
			
			<%
			} 
			%>
			
		</div>
	</div>

	<script src="https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js"></script>
	<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
</body>
</html>
5) replyAction.jsp 구현
```

### - replyAction.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="reply.ReplyDAO" %>
<%@ page import="java.io.PrintWriter" %>
<% request.setCharacterEncoding("UTF-8"); %>
<jsp:useBean id="reply" class="reply.Reply" scope="page"/>
<jsp:setProperty name="reply" property="replyContent"/>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width" initial-scale="1">
<title>JSP 게시판 웹 사이트</title>
</head>
<body>
	<%
		int bbsID=1;
		if(request.getParameter("bbsID")!=null){
			bbsID=Integer.parseInt(request.getParameter("bbsID"));
		}
	
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
			if(reply.getReplyContent()==null){
				PrintWriter script= response.getWriter();
				script.println("<script>");
				script.println("alert('댓글을 입력해주세요.')");
				script.println("history.back()");
				script.println("</script>");
			}
			else{
				ReplyDAO replyDAO=new ReplyDAO();
				int result = replyDAO.write(bbsID, reply.getReplyContent(), userID);
				if(result==-1){
					PrintWriter script= response.getWriter();
					script.println("<script>");
					script.println("alert('댓글쓰기에 실패했습니다.')");
					script.println("history.back()");
					script.println("</script>");
				}
				else{
					String url="view.jsp?bbsID="+bbsID;
					PrintWriter script= response.getWriter();
					script.println("<script>");
					script.println("location.href='"+url+"'");
					script.println("</script>");
				}
			}
		}
	%>
</body>
</html>
```

## 4) 글 수정 기능

[![image](https://user-images.githubusercontent.com/49560745/98539819-4ae51080-22d0-11eb-8c85-2ce0186a9b09.png)](https://user-images.githubusercontent.com/49560745/98539819-4ae51080-22d0-11eb-8c85-2ce0186a9b09.png)

```
1. update.jsp 구현

이전 글 제목, 내용 불러오기
이어서 글을 수정할 수 있도록한다.
-----------------------------------------------------------------------------------------
<td><textarea type="text" class="form-control" placeholder="글 제목" name="bbsTitle" maxlength="50"><%= bbs.getBbsTitle() %></textarea></td>
					</tr>
					<tr>
<td><textarea type="text" class="form-control" placeholder="글 내용" name="bbsContent" maxlength="2048"><%= bbs.getBbsContent() %></textarea></td>
-----------------------------------------------------------------------------------------
```

### - update.jsp

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
		
		int bbsID=1;
		if(request.getParameter("bbsID")!=null){
			bbsID=Integer.parseInt(request.getParameter("bbsID"));
		}
		
		BbsDAO bbsDAO=new BbsDAO();
		Bbs bbs=bbsDAO.getBbs(bbsID);
		
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
		
	</nav>
	<div class="container">
		<div class="row">
		<form method="post" action="updateAction.jsp?bbsID=<%= bbsID %>">
			<table class="table table-striped" style="text-align:center; border :1px solid #dddddd" > <%-- 홀,짝 행 구분 --%>
				<thead>
					<tr>
						<th colspan="2" style="background-color : #eeeeeee; text-align:center;">게시판 글수정 양식</th>
					</tr>
				</thead>
				<tbody>
					<tr>
						<td><textarea type="text" class="form-control" placeholder="글 제목" name="bbsTitle" maxlength="50"><%= bbs.getBbsTitle() %></textarea></td>
					</tr>
					<tr>
						<td><textarea type="text" class="form-control" placeholder="글 내용" name="bbsContent" maxlength="2048"><%= bbs.getBbsContent() %></textarea></td>
						
					</tr>
				</tbody>
			</table>	
			<input type="submit" class="btn btn-primary pull-right" value="글수정">
		</form>
		</div>
	</div>

	<script src="https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js"></script>
	<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
</body>
</html>
2. updateAction.jsp 구현
```

### - updateAction.jsp

```java
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
			int bbsID=1;
			if(request.getParameter("bbsID")!=null){
				bbsID=Integer.parseInt(request.getParameter("bbsID"));
			}
			System.out.println(bbsID);
			
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
					int result = bbsDAO.update(bbsID,bbs.getBbsTitle(),bbs.getBbsContent());
					if(result==-1){
						PrintWriter script= response.getWriter();
						script.println("<script>");
						script.println("alert('글수정에 실패했습니다.')");
						script.println("history.back()");
						script.println("</script>");
					}
					else{
						PrintWriter script= response.getWriter();
						script.println("<script>");
						script.println("alert('수정되었습니다.')");
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