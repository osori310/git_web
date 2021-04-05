# 자바 웹을 다루는 기술 리뷰

## 1. 웹 애플리케이션 이해하기

* **실습 1)**


**webShop 실습**


웹 애플리케이션 구성 요소의 기능


1. webShop 폴더 : 
    웹 애플리케이션의 루트 디렉터리.
    다른 웹 애플리케이션 이름과 중복을 허용하지 않으며, 여기에는 JSP HTML 파일이 저장됨


2. WEB-INF 폴더 : 
    웹 애플리케이션에 관한 정보가 저장되는 곳.
    이 디렉터리는 외부에서 접근할 수 없음


3. classes 폴더 : 
    웹 애플리케이션이 수행하는 서블릿과 다른 일반 클래스들이 위치하는 곳.


4. lib 폴더 : 
    웹 애플리케이션이 사용되는 여러 가지 라이브러리 압축 파일(.jar)이 저장되는 곳.
    DB 연동 드라이버나 프레임워크 기능 관련 jar 파일이 여기에 저장됨.
    lib 디렉터리의 jar는 클래스패스가 자동으로 설정됨


5. web.xml 파일 : 
    배치 지시자(deployment descriptor)로서 일종의 환경 설정 파일.
    웹 애플리케이션에 대한 여러 가지 설정을 할 때 사용됨


---

추가 기능들

6. jsp/html : 
    JSP 파일과 HTML 파일이 저장된 곳.


7. css :
    스타일시트 파일이 저장된 곳.


8. image :
    웹 애플리케이션에서 사용되는 이미지가 저장된 곳.


9. js : 
    자바스크립트 파일이 저장된 곳.


10. bin : 
    애플리케이션에서 사용되는 각종 실행 파일들이 저장된 곳.


11. conf : 
    프레임워크에서 사용하는 각종 설정 파일이 저장된 곳.


12. src :
    자바 소스 파일이 저장된 곳.

---

* webShop 폴더 -> WEB-INF 폴더 -> classes, lib 폴더 + web.xml 파일


    1. webShop 폴더를 tomcat9의 하위폴더 webapps에 복사 & 붙여넣기
    
    2. VScode 를 통해 main.html(웹애플리케이션)을 tomcat9의 하위폴더 webapps -> webShop 최상단에 생성
    
    3. tomcat9의 bin 폴더로 진입하여 startup 실행
    
    4. 톰캣 컨테이너가 실행되며 이후 브라우저에서 웹 애플리케이션 요청
    
           -> http://localhost:포트번호(8080)/컨텍스트이름/요청파일이름
           -> http://localhost:8080/webShop/main.html



* 컨텍스트


앞에서와 같이 webShop 프로젝트를 미리 webapps 폴더에 위치시켜 놓은 다음 톰캣을 실행했을 때 자동으로 webShop이 등록되어 실행되는 것을 확인


이와 같은 방법은 웹 애플리케이션 개발을 모두 완료한 후 사용자에게 서비스할 때 사용하면 편리


cf) 실제 개발 과정에서는 수시로 애플리케이션을 실행하고 테스트 해야함


- 실제로 개발할 때는 webShop을 만든 것처럼 개발자가 정한 위치에 웹 애플리케이션을 생성한 후 그 위치를 server.xml에 등록해 놓고, 톰캣을 실행하는 식으로 개발


- 톰캣은 server.xml에 입력된 정보에 따라 해당 위치로 이동하여 애플리케이션을 확인한 후 실행


- 이 때 server.xml에 등록하는 웹 애플리케이션을 `컨텍스트`라고 함.
    즉, 톰캣 입장에서 인식하는 한 개의 웹 애플리케이션


- 컨테이너 실행 시 웹애플리케이션당 하나의 컨텍스트 생성.
    - 일반적으로 컨텍스트 이름은 웹 애플리케이션과 같게 만드는 것이 일반적

    - 주요 특징)
        1. 웹 애플리케이션당 하나의 컨텍스트 등록
        
        2. 웹 애플리케이션 이름과 같을 수도 있고, 다를 수 있음
        
        3. 컨텍스트 이름은 중복되면 안됨
        
        4. 웹 애플리케이션의 의미를 가장 잘 나타낼 수 있는 명사형으로 지정
        
        5. 대소문자 구분
        
        6. server.xml에 등록
        
           


* 톰캣 컨테이너에 컨텍스트 등록하기


> 임의의 폴더에 만든 webShop 웹 애플리케이션을 server.xml에 컨텍스트로 등록해서 실행하는 과정 실습



- server.xml의 위치 : tomcat9 -> conf



- server.xml에 컨텍스트를 등록하려면 다음과 같이 <Context> 태그 이용

  ```xml
  <Context path = "/context name"
           docBase = "실제 웹 애플리케이션의 WEB-INF 디렉터리 위치"
           reloadable = "true or false" />
  ```

  1. path : 웹 애플리케이션의 컨텍스트 이름으로 웹 애플리케이션의 이름과 다를 수 있으며 웹 브라우저에서 실제 웹 애플리케이션을 요청하는 이름

  2. docBase : 컨텍스트에 대한 실제 웹 애플리케이션이 위치한 경로. 

     >  WEB-INF 상위 폴더까지의 경로

  3. reloadable : 실행 중 소스 코드가 수정될 경우 바로 갱신할 지 설정

     > 만약 false로 설정하면 톰캣을 다시 실행해야 추가한 소스 코드의 기능이 반영됨

  

  ex)

  ```xml
  <Context path = "/webMal"
           docBase = "C:\Users\power\git_web\webShop"
           reloadable = "true" />
  ```



	- 일반적으로 컨텍스트 이름은 웹 애플리케이션 이름과 동일하게 함
	- 하지만 이번 실습에서는 다르게 등록
	- 실제 웹 애플리케이션은 "C:\Users\power\git_web\webShop"에 있지만 webShop이 아닌 webMal이라는 이름으로 컨텍스트 등록



-> VScode를 통해 server.xml 파일을 열어 

```xml
 <Host name="localhost"  appBase="webapps"
            unpackWARs="true" autoDeploy="true">

	<!-- 이 부분 추가 -->
     <Context path = "/webMal"
         docBase = "C:\Users\power\git_web\webShop"
         reloadable = "true" />
	<!-- 이 부분 추가 -->
```



이후 tomcat9을 재실행 한 후 브라우저에 다음과 같이 입력

```
http://localhost:8080/webMal/main.html
```

cf) `webShop` 에서 `webMal`로 변경 



* 본 실습에서 톰캣 컨테이너에서의 웹 애플리케이션 동작 과정
  1. 웹 웹 브라우저에서 컨텍스트 이름(webMal)으로 요청
  2. 요청을 받은 톰캣 컨테이너는 요청한 컨텍스트 이름이 server.xml에 있는지 확인
  3. 해당 컨텍스트 이름이 있으면 컨텍스트 이름에 대한 실제 웹 애플리케이션이 있는 경로(C:\Users\power\git_web\webShop)로 가서 요청한 main.html을 클라이언트 웹 브라우저로 전송
  4. 웹 브라우저는 전송된 main.html을 브라우저에 나타냄



* **실습2)**

지금까지 웹 애플리케이션이 실행되는 과정을 쉽게 이해하기 위해 직접 웹 애플리케이션을 만들어 실습해 보았음

이제 이클립스(개발 도구)를 이용하여 웹 애플리케이션을 만들고, 톰캣 컨테이너에 등록한 후 실행하는 과정알아보기



1. 이클립스에서 웹 프로젝트 생성

   * 이클립스에서 한 개의 프로젝트가 한 개의 웹 애플리케이션
   * 즉, 프로젝트 이름이 웹 애플리케이션 이름
   * 이클립스 실행 후 Project Explorer 영역에서 마우스 오른쪽 버튼을 클릭한 후 New > Dynamic Web Project 선택

2. 이름 : webShop & next, next 후 web.xml 생성할 것인지 묻는 체크박스에 체크 & Finish

3. Project Explorer에 webShop 프로젝트가 생성된 것을 확인

4. 이클립스에서 HTML 파일 생성

   1. 프로젝트 하위 메뉴에서 WebContent를 선택하고 마우스 오른쪽 버튼 클릭으로 New > HTML File 선택

   2. 이름 : main.html으로 생성

   3. 다음과 같이 간단한 HTML 코드를 작성한 후 저장

      ```html
      <!DOCTYPE html>
      <html>
      <head>
      <meta charset="UTF-8">
      <title>Hello JSP</title>
      </head>
      <body>
      Hello JSP!!
      안녕하세요!!
      </body>
      </html>
      ```

5. 이클립스에서 만든 프로젝트를 톰캣 컨테이너에 등록한 후 실행

   1. 이클립스 하단의 Servers 탭을 선택하고 마우스 오른쪽 버튼을 클릭한 후 New > Server를 선택
   2. Apache 항목에서 Tomcat v9.0 Server를 선택한 후 Next
   3. 톰캣 설치 디렉터리 선택

6. 이클립스와 연동한 톰캣에 프로젝트 등록

   -> 앞에서 server.xml에 직접 등록했었지만 eclipse에서 더 간단하게 설정할 수 있음

   1. Servers 탭 아래에 등록된 톰캣 서버 Tomcat v9.0 Server at localhost [Stopped]를 선택한 후 마우스 오른쪽 버튼을 클릭하여 Add and Remove를 선택
   2. Available에서 실습에서 만든 프로젝트명(webShop)를 선택한 후 Add> 클릭
   3. Finish

7. Servers > server.xml에 다음과 같이 추가된 내용을 확인

   ```xml
   <Context docBase="webShop" path="/webShop" reloadable="true" source="org.eclipse.jst.jee.server:webShop"/></Host>
   ```

8. 웹 브라우저에서 요청하기

   1. Servers 탭 오른쪽에 있는 녹색 실행버튼을 클릭해 서버 실행

   2. 브라우저에서 다음 주소로 요청

      ```
      http://localhost:8080/webShop/main.html
      ```

   

* 웹 애플리케이션 서비스하기

> 이클립스에서 프로젝트를 만들어 톰캣 컨테이너에서 실행
>
> -> 이클립스에서 개발한 웹 애플리케이션을 실제 사용자에게 서브사힉 위해 배치(deploy)하는 방법에 대해 알아봄



- 배치(deploy)란?

  이클립스에서 개발할 경우 개발자 입장에서 자신이 만든 기능이 정상적으로 실행되는지 확인하기 위해 빈번하게 톰캣을 재실행하곤 함

  -> 이러한 개발 과정을 거쳐 애플리케이션이 완성되면 실제 사용자들에게 서비스를 해야하는데 이 단계에서 이클립스에 등록된 톰캣에서 실행하는 것은 무의미

  --> 따라서 실제로 리눅스나 유닉스 서버에 설치된 톰캣으로 실행해야 함

  이렇게 하려면 이클립스에서 개발한 웹 애플리케이션 예제 소스 전체를 실제로 서비스하는 톰캣으로 이동하여 실행해야 함 -> deploy 과정



* 톰캣에 배치하기
  * 개발을 마친 후 프로젝트를 war 압축 파일로 만든 후 FTP를 이용해 톰캣이 미리 설치된 리눅스나 유닉스 같은 운영 서버에 업로드
  * 이후 텔넷(telnet)을 이용해 bin 폴더의 Tomcat.exe.를 다시 실행하면 톰캣 실행 시 war 파일의 압축이 해제됨과 동시에 자동으로 등록되어 웹 애플리케이션이 실행됨

  

  1. 이클립스 상단 메뉴에서 File > Export 선택

  2. Web 항목의 WAR file을 선택한 후 Next

  3. Browse를 클릭해 war 파일을 저장할 위치 지정

  4. 톰캣 폴더의 webapps 디렉터리를 지정하고 webShop.war 파일 이름으로 저장

  5. webapps 폴더에 webShop.war 파일이 생성된 것을 확인

  6. 톰캣 루트 디렉터리 하위의 bin 폴더에서 startup 클릭하여 실행

  7. webapps 폴더에 webShop 폴더가 생성된 것을 확인

  8. webShop 폴더에서 WEB-INF 폴더와 main.html 파일 확인

  9. 웹 브라우저에서 다음과 같이 컨텍스트 이름으로 요청

     ```
     http://localhost:8080/webShop/main.html
     ```

* 톰캣의 webapps 폴더에 위치하는 웹 애플리케이션은 직접 server.xml에 등록하지 않아도 톰캣 실행 시 자동으로 등록됨

* 따라서 webShop.war를 미리 webapps 폴더에 위치시킨 후 톰캣을 실행하면 톰캣이 알아서 압축을 해제한 후 생성된 웹 애플리케이션을 자동으로 등록



# 2. 서블릿 이해하기

