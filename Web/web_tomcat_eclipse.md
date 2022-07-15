# Tomcat 설치 후 Eclipse Dynamic Web Project 연결

> Tue Jul 5, 2022

---

[toc]

웹페이지에서 데이터베이스를 쓰지 않는 경우는 거의 없습니다.

페이지 하나를 구현하기 위해서는 

* html / css / javascript / jquery / ajax / bootstrap (프론트엔드)
* jsp / spring / mybatis / java (백엔드)



Java 로 만드는 서버페이지 JSP 는 큰 프로젝트에서 많이 사용됩니다.

PHP는 무료라 쇼핑몰 같은 소규모 프로젝트에 많이 사용됩니다.

JSP 가 실행되는 Tomcat 같은 서버가 필요하며 PHP 는 Apache 가 필요합니다.

html 파일은 jsp 를 포함시킬수 없으며 반대는 가능합니다.



HTML 은 우리가 보는 페이지의 구도를 잡는다고 생각하면 좋습니다.

기둥, 천장은 HTML. 집의 가구나 인테리어는 CSS 라고 생각하시면 좋습니다.



### Tomcat 서버 연결

우선 Tomcat 을 다운받고 아래를 터미널에서 입력합니다.

```bash
# Apache Tomcat을 다운받은 경로에서 /usr/local 경로로 압축 해제
sudo tar -xzvf apache-tomcat-9.0.64.tar.gz -C /usr/local/apache-tomcat-9.0.64

# 혹은 다운받은 폴더에서 /usr/local 로 이동
sudo mv apache-tomcat-9.0.64 /usr/local

# Library 경로에 Tomcat 심볼릭 링크 추가
sudo ln -s /usr/local/apache-tomcat-9.0.64 /Library/Tomcat  

# Tomcat 설치 디렉토리 및 파일 소유자 변경 
sudo chown -R sanghyeop /Library/Tomcat  

# Tomcat 실행/종료 스크립트의 실행 권한 추가
sudo chmod +x /Library/Tomcat/bin/*.sh 
```



톰캣을 실행하거나 종료합니다.

```bash
# Apache Tomcat 실행
sudo /Library/Tomcat/bin/startup.sh

# 크롬에서 http://localhost:8080 입력후 접속 확인

# Apache Tomcat 종료
sudo /Library/Tomcat/bin/shutdown.sh
```



이클립스에서 가상 서버를 만들어 가져와서 사용합니다. 그러기위해 Tomcat 을 종료시켜야 합니다.





### Tomcat 으로 프로젝트 배포하기

개발이 완료된 프로젝트를 배포하는 방법

프로젝트 하나가 사이트 하나입니다.

1. 이클립스에서 프로젝트를 war 파일로 압축합니다.
2. 서버에 webApps 에 복사를 합니다.
3. 서버를 start 하면 압축이 알아서 풀립니다. 

