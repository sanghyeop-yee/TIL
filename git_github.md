# Git & Github

> Wed Jun 8, 2022

---

![inCaseOfFire](git_github.assets/inCaseOfFire.png)

[toc]

## Git 에 기록을 남기는 순서

* `git init`
  * 깃으로 관리를 시작
* .gitignore 파일 만들기
* 파일에 변경사항을 주기 (새로 만들기, 수정하기, etc)
* `git add {파일 이름}` 또는 `git add .`
  * staging area 에 올라갑니다.
  * `git rm --cached {filename}` 으로 add 된 파일을 지울수 있습니다.
* `git commit -m "코밋 메세지"`
  * commit (기록) 을 남깁니다.
* `git log` 또는 `git log --oneline` 
  * 커밋 확인
* `git remote add origin {github repository url}`
  * github repository 생성하여 로컬과 원격 저장소 연결

* `git push origin master` 또는`git push -u origin master`
  * 원격 저장소에 커밋 업로드
* 원격 저장소에서 정상 업로드 확인



### .gitignore 파일로 git 관리 방지하기

> 비밀번호 같이 git 으로 관리하지 않을 파일들은
>
> .gitignore 파일을 만들어서 그 안에 파일 이름 나열하기

* [toptal.io](gitignore.io) 를 활용하여 .gitignore 에 붙여넣을수도 있습니다.
* `git init` 을 하자마자 미리 만들어놔야 합니다.



### 주의사항

* Github 원격 저장소에 절대로 파일을 드래그해서 업로드 하지 않습니다.
* .gitignore 파일을 `git init` 을 하자마자 미리 만듭니다.



## Git Clone / Pull 하기

> Github 에 있는 Repository 를 옮겨오기
>
> ** 처음 가져올때는 **Clone** 이후에는 **Pull**

* Github Repository 가서 HTTPS 주소 복사하기
* `git clone {https url}`
  * 새로운 로컬 폴더를 만들었을때
* `git clone {https url} .` 
  * 현재 로컬 폴더에 옮겨오고자 할때
* `git clone {https url} filename` 
  * 새로운 폴더 이름으로 가져올때
* `git pull`
  * 이미 clone 한 폴더에 추가된 파일 가져오기



### Conflict 한 상황이라면?

* `git pull` 을 먼저 실행해서 땡겨옵니다.
  * Merge branch 가 실행됩니다.
* `git push` 로 추가된 파일을 업로드 합니다.



## 끝말잊기 하기

> 혼자서 끝말잊기를 진행해봅시다.

* word_relay.md 만들기
* add - commit - push
* 원격 저장소랑 연결
* 완전 다른 폴더에 해당 repo 를 clone



* 단어 적기
* add - commit - push
* 반대쪽에서 pull
* 단어 적어 -> add - commit - push
* 반대쪽에서 pull