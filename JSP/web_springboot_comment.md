# Springboot: 게시판 댓글

> Fri Aug 5, 2022

---





이번에는 게시판에 댓글을 다는 방법을 알아봅시다. Ajax 를 활용해서 만들어 봅시다.

댓글을 저장하기 위해서는 어떤 데이터가 필요할까요?

우선 일련번호, 글쓴이 (로그인 한 사람), 쓴 날짜, 내용이 필요하고 댓글을 단 게시물 번호가 반드시 있어야 합니다.



먼저 DB 를 설계하겠습니다. 테이블 이름은 ReplyBoard 로 지정하여 필요한 컬럼들을 만들겠습니다. 원래 글이 지워지면 안에 있던 댓글도 함께 지워지도록 설정 (cascade) 하겠습니다.



![image-20220805161758673](web_springboot_comment.assets/image-20220805161758673.png)



boardView.jsp 로 돌아와 댓글 폼을 작성합니다.

```jsp
	<!-- 댓글 -->
	<div>
		<form method="post" id="replyFrm">
			<!-- 원글 글번호 -->
			<input type="hidden" name="no" value="${vo.no }"/>
			<textarea maxlength="100" name="comment" id="comment" cols="50" rows="3"></textarea>
			<input type="submit" value="댓글쓰기"/>
		</form>
	</div>
```



이어서 댓글 리스트를 작성합니다.

```jsp
```





























