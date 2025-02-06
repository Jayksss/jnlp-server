## **목차**
1. [JNLP 실행 문제 및 해결 방법](#1-jnlp-실행-문제-및-해결-방법)
2. [JAR 파일 서명 및 검증](#2-jar-파일-서명-및-검증)
3. [CMD에서 JAR 파일 실행 및 아규먼트 전달](#3-cmd에서-jar-파일-실행-및-아규먼트-전달)
4. [Java Web Start (javaws) 명령어 사용](#4-java-web-start-javaws-명령어-사용)

---

## **1. JNLP 실행 문제 및 해결 방법**

### **(1) JNLP 실행 오류: "지정된 파일 URL을 로드할 수 없음"**
- **원인:**  
  - JNLP의 `codebase`나 `href` 경로가 잘못되었거나, 네트워크 문제로 파일을 찾지 못하는 경우.
  - 웹 서버가 `application/x-java-jnlp-file` MIME 타입을 제대로 제공하지 않는 경우.

- **해결 방법:**  
  1. JNLP 파일의 `codebase`와 `href`가 제대로 설정되었는지 확인한다:
     ```xml
     <jnlp spec="1.0+" codebase="http://localhost:8080/MyWebProject" href="myapp.jnlp">
     ```

  2. 웹 서버에서 **MIME 타입**이 제대로 설정되었는지 확인한다:
     ```xml
     <mime-mapping>
         <extension>jnlp</extension>
         <mime-type>application/x-java-jnlp-file</mime-type>
     </mime-mapping>
     ```

  3. 브라우저에서 **JNLP 파일과 JAR 파일**이 정상적으로 접근되는지 테스트한다.

---

### **(2) JNLP 실행 시 HTTPS 사용 권장**
- Java 8 이상에서는 JNLP와 JAR 파일이 **HTTP**로 제공되면 보안 경고나 실행 오류가 발생할 수 있다.
- **해결 방법:**  
  JNLP 및 JAR 파일을 **HTTPS**로 제공하거나, **Java 보안 설정**에서 예외 사이트로 추가한다:
  - Java 제어판 → 보안(Security) 탭 → 예외 사이트 목록에 URL 추가
  - 없을 시 수동 실행 => C:\Program Files\Java\JRE_8u202\bin\javacpl.exe

---

## **2. JAR 파일 서명 및 검증**

### **(1) 서명 키 생성**
```bash
keytool -genkeypair -alias mykey -keyalg RSA -keysize 2048 -validity 365 -keystore mykeystore.jks

jarsigner -keystore mykeystore.jks MyApp.jar mykey
jarsigner -verify MyApp.jar
```

### **3. CMD에서 JAR 파일 실행 및 아규먼트 전달**
```bash
java -jar MyApp.jar arg1 arg2 arg3 arg4
```

### **4. Java Web Start (javaws) 명령어 사용**

### **(1) JNLP 파일 실행**
```bash
javaws "C:\path\to\myapp.jnlp"
```

### **(2) -wait 옵션 사용**
```bash
javaws -wait "http://localhost:8080/MyWebProject/myapp.jnlp"
```
