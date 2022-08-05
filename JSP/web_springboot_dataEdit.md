# Springboot: 자료실 글 수정

> Fri Aug 5, 2022

---



이번에는 자료실 글 수정을 하는 방법을 배워봅시다.

자기가 쓴 글에만 수정을 해야겠죠?

dataView.jsp 

```jsp
	<div>
		<!-- 본인 쓴 글일경우 수정, 삭제 -->
		<c:if test="${dataVO.userid==logId}">
			<a href="/data/dataEditForm/${dataVO.no}">수정</a>
		</c:if>
	</div>
```



DataController.java 로 와서 글수정 폼으로 가는 매핑을 작업합니다.

```java
	// 글수정 폼
	@GetMapping("dataEditForm/{no}")
	public ModelAndView dataEditForm(@PathVariable("no") int no) {
		mav = new ModelAndView();
		mav.addObject("vo", service.dataSelect(no));
		mav.setViewName("data/dataEditForm");
		return mav;
	}
```



그렇다면 이제 data/dataEditForm.jsp 파일을 만들어야 합니다. 수정을 하면 data/dataWrite 의 형태로 돌아간다는 뜻이니까 data/dataWrite 를 save as 로 해서 내용만 조금 수정하겠습니다.

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<style>
	.container li{border-bottom:1px solid #ddd;}
</style>
<script>
	$(function(){
		$(".down a").click(function(){
			$.ajax({
				url : "/data/downCountUpdate",
				data : "no=${dataVO.no}",
				success : function(result){
					
				}, error : function(e){
					console.log(e.responseText);
				}
			});
		});
	});
</script>

<div class="container">
	<h1>글내용보기</h1>
	<ul>
		<li>번호</li>
		<li>${dataVO.no }</li>
		<li>작성자</li>
		<li>${dataVO.userid }</li>
		<li>조회수 : ${dataVO.hit }, 다운횟수 : <span id="dCount">${dataVO.downcount }</span>, 등록일 : ${dataVO.writedate }</li>
		<li>첨부파일:
				<a href="/upload/${dataVO.filename1 }" download>${dataVO.filename1 }</a>
				<c:if test="${dataVO.filename2!='' }">
					, <a href="/upload/${dataVO.filename2 }" download>${dataVO.filename2 }</a>
				</c:if>
		</li>
		<li>제목</li>
		<li>${dataVO.title }</li>
		<li>글내용</li>
		<li>${dataVO.content }</li>
	</ul>
	<div>
		<!-- 본인 쓴 글일경우 수정, 삭제 -->
		<c:if test="${dataVO.userid==logId}">
			<a href="/data/dataEditForm/${dataVO.no}">수정</a>
		</c:if>
		
		삭제	
	</div>
</div>
```



인터셉터가 처리될 매핑주소를 추가적으로 작성합니다.

```java
package com.cali.myapp;

import java.util.Arrays;
import java.util.List;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

import com.cali.myapp.interceptor.LoginInterceptor;

@Configuration
public class ServerConfigure implements WebMvcConfigurer {
	// 인터셉터가 처리될 매핑주소를 List 컬렉션으로 작성한다. 
	private static final List<String> URL_PATTERNS = Arrays.asList(
			"/member/memberEdit", "/member/memberEditOk", 
			
			"/board/boardForm", "/board/boardForm", "/board/boardFormOk", "/board/boardEdit",
			"/board/boardEditOk", "/board/boardDel", "/board/multiDel",
			
			"/data/dataWrite", "/data/dataWriteOk",
			"/data/dataEditForm/*", "/data/dataEditFormOk",
			"/data/dataDel"
			
			);
	
	@Override
	public void addInterceptors(InterceptorRegistry registry) {
		registry.addInterceptor(new LoginInterceptor()).addPathPatterns(URL_PATTERNS);
	}
}
```





이번에는 첨부파일을 삭제하는 방법을 알아봅시다.

현재 우리가 올릴 수 있는 첨부파일은 2개로 제한했습니다.

두개일때 한개일때 보여지는게 달라야하겠죠? 

첨부파일이 한개일때 삭제를 하러 들어가면 하나는 첨부파일을 추가할 수 있고 하나는 기존 첨부파일 이름과 함께 지울 수 있는 기능이 있을겁니다.

```jsp
		<ul>
			<li>제목</li>
			<li><input type="text" name="title" style="width:80%" value="${vo.title }"></li>
			<li>글내용</li>
			<li><textarea name="content" id="content">${vo.content }</textarea></li>
			<li>첨부파일</li>
			<li>
				<!-- 첫번째 첨부파일 -->
				<div>
					<!-- 현재 있는 파일명 -->
					<div>${vo.filename1 }&nbsp;<b>X</b></div>
					<!-- 파일명을 삭제시 새로운 파일명을 첨부할 수 있도록 -->
					<input type="hidden" name="filename" id="filename1"/>
					<!-- 삭제한 경우 삭제 파일명을 보관할 컴퍼넌트 -->
					<input type="hidden" name="" value="${vo.filename1 }"/>
				</div>
				<!-- 두번째 첨부파일 -->
				<div>
					<!-- 첨부파일 있을때 -->
					<c:if test="${vo.filename2!=null && vo.filename2!='' }">
						<div> ${vo.filename2 }&nbsp;<b class="del">X</b></div>
						<input type="hidden" name="filename" id="filename2"/>
						<input type="hidden" name="" value="${vo.filename2 }"/>
					</c:if>
					<!-- 첨부파일 없을때 -->
					<c:if test="${vo.filename2==null || vo.filename2=='' }">
						<input type="file" name="filename" id="filename2"/>
					</c:if>
					
				</div>
				
			</li>
			<li><input type="submit" value="자료실 글 수정하기"></li>
		</ul>
```



스크립트 파트로 돌아와서 추가해줍니다.

```jsp
<script>
	$(function(){
		CKEDITOR.replace("content");
		// 첨부파일 수정
		$(".del").click(function(){
			// 기본 파일명 숨기기
			$(this).parent().css("display", "none");
			// 새로 첨부가능하도록 파일컴포넌트 만들어주기
			$(this).parent().next().attr("type", "file");
			// 삭제된 파일명을 서버로 보내기 위해서 name 속성을 설정한다.
			$(this).parent().next().next().attr("name", "delFile");
		});
```





다음은 DataVO 로 와서 삭제한 파일명이 보관될 컬렉션을 생성해줍니다.

```java
private List<String> delFile; // 삭제한 파일명이 보관될 컬렉션
```



