https://youtu.be/VAp0n9DmeEA

## 1. Maven 기능
 - 프로젝트 생성     ->  사용자 정의 프로젝트
 - 라이브러리 설정   -> 라이브러리 관리와 의존성 체크
 - 코드 작업
 - 컴파일
 - 테스트
 - 패키지 만들기
 - 인스톨
 - 배포             -> 라이브러리 저장소 활용
 - 레포팅
#

## 2. Maven 설치
 - http://maven.apache.org
    - Use -> Download -> Files -> Binary zip archive
 - 환경변수(path) 등록 
    - C:\java\apache-maven-3.6.3\bin;

 - command 창에 mvn -version 으로 확인
#

## 3. Java 프로젝트 생성
- mvn archetype:generate -DgroupId=com.newlecture -DartifactId=javaprj -DarchetypeArtifactId=maven-archetype-quickstart


1. archetypeArtifactId   
    - 다른사람이 만든 maven-archetype-quickstart 라는 프로젝트 구조를 기본으로 생성하겠다.

2. artifactId
    - javaprj 라는 프로젝트를 생성한다.
3. groupId
    - 프로젝트를 식별하기 위한 그룹명은 com.newlecture 한다
    - 큰 범위부터 뒤집어서 주로 도메인을 사용

4. mvn archetype:generate
    - 새로운 아키텍처 타입을 만든다 ? 
 

#

## 4. 컴파일과 실행하기
 - pom.xml 파일 위치에서 
    - mvn compile : 컴파일
    - mvn package : packaging에 명시되어 있는 타입으로 파일생성

 - pom.xml java 버전 수정
    ```xml
    <properties>
        <!-->컴파일을 1.8로 하겠다<-->
        <maven.compiler.source>1.8</maven.compiler.source>
        <!-->대상을 최소 1.8 이상에서 컴파일 하겠다.<-->
        <maven.compiler.target>1.8</maven.compiler.target>
    </properties>
    ```

 - 패키징된 jar 실행
    - java -cp target\javaprj-1.0-SNAPSHOT.jar com.newlecture.App
        - cp : classspath 
        - target\javaprj-1.0-SNAPSHOT.jar (상대경로)    
          com.newlecture.App 클래스가 포함되어 있는 라이브러리 추가

#

## 5. Build Lifecycle

- compile, test, package, install 같은 명령어로 실행시 
  바로 전 단계까지 실행 되며 각 단계는 플러그인 방식 이여서 처럼 꼈다 뺐다 할 수 있다.

- 각 단계별로 플러그인 방식 으로 끊어놔서    
  각 단계 마다 실행하는 프로그램(소프트웨어(플러그인))이 따로 있다. 
  빌드 단계를 실행하는 플러그인 소프트웨어들 안에는 goal 이라는 단계가 있다 

-  Phase -> Plug-in -> Goal(내부적인으로 나뉘어진 프로그램 단계?)       
   (Phase:Plug-in:Goal -> compile: org.apache.maven.plugins:maven-compiler-plugin:3.1:compile)  
  ```xml
    <phases>
        <phase>validate</phase>
        <phase>initialize</phase>        
        <phase>generate-sources</phase>                
        <phase>process-sources</phase>
        <phase>generate-resources</phase>                
        <phase>process-resources</phase>        
        <phase>compile</phase>        
        <phase>process-classes</phase>
        <phase>generate-test-sources</phase>                      
        <phase>process-test-sources</phase>           
        <phase>generate-test-resources</phase>                   
        <phase>process-test-resources</phase>                           
        <phase>test-compile</phase>    
        <phase>process-test-classes</phase>    
        <phase>test</phase> 
        <phase>prepare-package</phase> 
        <phase>package</phase>                                       
        <phase>pre-integration-test</phase>        
        <phase>integration-test</phase>                
        <phase>post-integration-test</phase>
        <phase>verify</phase>
        <phase>install</phase>
        <phase>deploy</phase>        
    </phases>
<!-->
POM.xml 파일 packaging  에 따라 단계(phase) 는 달라질수 있다.
    plugin bindings for pom packaging
    plugin bindings for jar packaging
    plugin bindings for ejb packaging
    plugin bindings for maven-plugin packaging
    plugin bindings for war packaging
    plugin bindings for ear packaging
    plugin bindings for rar packaging                        
<-->    
```

#

## 6. 이클립스 IDE로 Maven 프로젝트 임포트 하기

- 기 생성된 Maven 프로젝트 Import
    - File -> Import -> Existing Maven Projects -> pom.xml 파일이 있는 폴더 지정    
#

## 7. 컴파일러 플러그인과 JDK 버전 변경

![mvn_help_plugin_goal](.\img\mvn_help_plugin_goal.png)
- compiler plugin 속성 변경 (오버라이드)
    ```xml
    <build>
        <plugins>
            <plugin>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.1</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>  				
                </configuration>
            </plugin>
        </plugins>
    </build>
    ```
- maven-compiler-plugin 버전 3.6 이상 부터는 오버라이드 안하고 properties 로 처리할 수 있다. 
    ```xml
    <properties>
        <!-->컴파일을 1.8로 하겠다<-->
        <maven.compiler.source>1.8</maven.compiler.source>
        <!-->대상을 최소 1.8 이상에서 컴파일 하겠다.<-->
        <maven.compiler.target>1.8</maven.compiler.target>
    </properties>
    ```
- pom.xml 파일 변경 후 Update Project 해주자.  
- 명령어 "mvn archetype:generate" 로 파라메터 없이 실행하면 생성되어 있는 아키타입을 볼 수 있으며       
  선택해서 생성할 수 있다.      
  (maven-archetype-quickstart 는 기본 설정으로 생성임)

#

## 8. 웹 프로젝트로 변경 (Java 프로젝트를 Web 프로젝트로 변경)
- Java 프로젝트 Web 프로젝트로 변경
    - pom.xml 파일에 packaging "jar" -> "war" 로 변경
    - webapp 홈 디렉토리 생성 확인      
    - web.xml 파일 없어서 오류 표시     
        - webapp 하위에 "WEB-INF" 폴더 생성 후 web.xml 파일 생성
        - WEB-INF 하위에 index.html 생성 후 서버 기동해서 테스트    
#

## 9. 라이브러리 설정하기   
- 로컬 Maven lib 저장소 위치 : ${user.home}/.m2/repository
- JSP 파일 추가시 오류  
  The superclass "javax.servlet.http.HttpServlet" was not found on the Java Build Path 오류     
    - Maven 사용전 : tomcat 서버 라이브러리 추가    
        - Java Build Path -> Libraries 탭 -> Add Library -> Server Runtime -> Tomcat 선택   
    - Maven 으로 처리   
        - 라이브러리 검색 : https://mvnrepository.com/ (검색 : jsp tomcat)
        - pom.xml 에 dependency 추가
        ```xml
        <!-- https://mvnrepository.com/artifact/org.apache.tomcat/tomcat-jsp-api -->
        <dependency>
            <groupId>org.apache.tomcat</groupId>
            <artifactId>tomcat-jsp-api</artifactId>
            <version>9.0.45</version>
        </dependency>
        ```
-  종속되어 있는 Dependency를 알아서 다운로드 함 pom.xml Dependency Hierarchy 탭에서 확인 가능      
   (디펜던시를 추가하면 종속된 라이브러리도 다 가져온다)
#

## 10. 라이브러리 오류 문제
- 라이브러리가 다운로드 되다가 깨지거나 락이 걸리면 문제가 발생함   
- 이클립스에 !(느낌표)는 라이브러리가 없을때 발생함     
- 로컬 Maven lib 저장소 위치 : ${user.home}/.m2/repository 하위 파일 전부 다 삭제 IDE툴은 다 종료   
- IDE 툴 다시 실행하면 다시 다운로드 함     
#

## 11. 라이브러리 인덱싱 검색 (IDE 툴에서 검색해서 라이브러리 추가) 
- pom.xml -> Dependencies 탭 -> Add -> 검색해서 추가 
- Index 빌드를 안했으면 검색이 안된다.  
  검색이 안되면 Maven Repositories -> Global Repositories -> Rebuild Index  수행
#

## 12. mvn install : 내가 만든 라이브러리 설치하기
- mvn install 명령어는 프로젝트가 지정된 packaging 으로 말아지고 로컬 저장소로 떨궈진다.
- mvn deploy 명령어는 원격 저장소로 배포함.
#

## 99. 부록
- 도움말 보기   
    - compile 명령어  : mvn help:describe -Dcmd=compile
        ```
        C:\devbro\study\maven test\javaprj>mvn help:describe -Dcmd=compile
        [INFO] Scanning for projects...
        [INFO]
        [INFO] -----------------------< com.newlecture:javaprj >-----------------------
        [INFO] Building javaprj 1.0-SNAPSHOT
        [INFO] --------------------------------[ jar ]---------------------------------
        [INFO]
        [INFO] --- maven-help-plugin:3.2.0:describe (default-cli) @ javaprj ---
        [INFO] 'compile' is a phase corresponding to this plugin:
        org.apache.maven.plugins:maven-compiler-plugin:3.1:compile

        It is a part of the lifecycle for the POM packaging 'jar'. This lifecycle includes the following phases:

        (Phase:Plug-in:Goal)

        * validate: Not defined
        * initialize: Not defined
        * generate-sources: Not defined
        * process-sources: Not defined
        * generate-resources: Not defined
        * process-resources: org.apache.maven.plugins:maven-resources-plugin:2.6:resources
        * compile: org.apache.maven.plugins:maven-compiler-plugin:3.1:compile
        * process-classes: Not defined
        * generate-test-sources: Not defined
        * process-test-sources: Not defined
        * generate-test-resources: Not defined
        * process-test-resources: org.apache.maven.plugins:maven-resources-plugin:2.6:testResources
        * test-compile: org.apache.maven.plugins:maven-compiler-plugin:3.1:testCompile
        * process-test-classes: Not defined
        * test: org.apache.maven.plugins:maven-surefire-plugin:2.12.4:test
        * prepare-package: Not defined
        * package: org.apache.maven.plugins:maven-jar-plugin:2.4:jar
        * pre-integration-test: Not defined
        * integration-test: Not defined
        * post-integration-test: Not defined
        * verify: Not defined
        * install: org.apache.maven.plugins:maven-install-plugin:2.4:install
        * deploy: org.apache.maven.plugins:maven-deploy-plugin:2.7:deploy

        [INFO] ------------------------------------------------------------------------
        [INFO] BUILD SUCCESS
        [INFO] ------------------------------------------------------------------------
        [INFO] Total time:  2.005 s
        [INFO] Finished at: 2021-08-11T14:39:15+09:00
        [INFO] ------------------------------------------------------------------------    
        ```
- 각 단계에서 사용될 수 있는 플러그인 확인
    - maven.apache.org -> Maven plugins -> 