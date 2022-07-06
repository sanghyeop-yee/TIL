# HTML Basics : 폼 태그

> Wed Jul 6, 2022

---

[toc]

### 폼 태그 : 클라이언트측에서 서버로 정보를 보낼 준비

name 이 데이터를 가져올수 있는 변수의 역할을 하며 이는 매우 중요합니다.

value 는 서버로 보내는 데이터이며 역시 매우 중요합니다.

Check box 는 여러개의 선택항, Radio button 하나의 선택을 할때 사용합니다. 

type=submit 으로 누르면 action 의 write.jsp 로 이동하여 jsp 의 기능을 수행합니다.

```html
<h1>클라이언트측에서 서버로 정보를 보낼 준비를 한다.</h1>
<pre>
	method : 전송방식 (get, post: 데이터를 숨겨서 보내는 방식)
	action : 서버페이지 파일명
</pre>
<form method="post" action="write.jsp">
	<ul>
		<li><label>Id, Name</label> : <input type="text" name="userid" size="30" value="" maxlength="10" placeholder="Enter user id" required/></li>
		<li>Password : <input type="password" name="userpwd" placeholder="Enter password"/></li>
		<li>Attach files : <input type="file" name="filename"/> </li>
		<li>Check box : <input type="checkbox" name="hobby" value="Tracking"/> Tracking
					 <input type="checkbox" name="hobby" value="Shopping" checked/> Shopping
					 <input type="checkbox" name="hobby" value="Game"/> Game
					 <input type="checkbox" name="hobby" value="Cycle" checked/> Cycle
					 
		</li> 
		<li>Radio box : <input type="radio" name="gender" value="M"/> Male
						<input type="radio" name="gender" value="F" checked/> Female
		
		</li>
		<li>Interest : <select name="interest" size="4" multiple>
							<option>Reading</option>
							<option>Movie</option>
							<option>Running</option>
							<option>Coding</option>
							<option>Surfing</option>
							<option>Music</option>
					   </select> 
		</li>
		<li><input type="submit" value="submit"/></li>
	</ul>
</form>
```





### optgroup 태그

```html
		<li>
			optgroup 태그 : <select>
								<optgroup label="Front-end">
									<option>HTML5</option>
									<option>CSS3</option>
									<option>JAVASCRIPT</option>
									<option>JQUERY</option>
								</optgroup>
								<optgroup label="Back-end">
						   			<option>JSP</option>
						   			<option>SPRING</option>
						   		</optgroup>
						   </select>
		
		</li>
```



### 여러줄의 문장

```html
		<li>
			여러줄의 문장 : <textarea name="intro" cols="50" rows="5">write here</textarea>
		</li>
```



### 날짜

```html
		<li>Date : <input type="date" name="date1"/></li>
		<li>Datetime-local : <input type="datetime-local" name="date2"/></li>
		<li>Month : <input type="month" name="date3"/></li>
		<li>Week : <input type="week" name="date3"/></li>
```



### 색/숫자

```html
		<li>Color : <input type="color" name="color"/></li>
		<li>Number : <input type="number" name="num" min="0" max="20" step="5" value="10"/></li>
		<li>Range : <input type="range" name="num2" min="1" max="10" step="2" value="3"></li>
```



### Submit

type=submit 으로 누르면 action 의 write.jsp 로 이동하여 jsp 의 기능을 수행합니다.

submit 과 button 은 같은 모양이 나오지만 submit 만 실제 action 을 취합니다.

```html
		<li>
			<input type="submit" value="submit"/>
			<input type="button" value="submit"/>
			<input type="reset" value="reset"/>
			<input type="image" src="../img/button.png"/>
		</li>
```

