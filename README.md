# node-java-study

### 사용할 npm 모듈

[java](https://www.npmjs.com/package/java)

### node 프로젝트

노드 버전 : 10.16.0

자바 버전 : 1.8 (openjdk-7-jre가 아닌 openjdk-7-jdk 패키지가 필요함.)

## 설치 방법

1. cmd 관리자 권한으로 들어간 후 해당 프로젝트의 경로를 가서 설치해준다.

```
npm install -g node-gyp
npm install --global --production windows-build-tools 
npm update // 필요시 생략가능
npm install java

```

1. yarn 으로 설치하는 방법

```
yarn global add node-gyp
yarn global add --production windows-build-tools 
yanr upgrade // 필요시 생략가능
yarn add java


```
## Spring Boot와 node 프로젝트의 방향성



1. npm i java 모듈에 대한 검증 테스트

    ```jsx
    var java = require('java');
    var javaLangSystem = java.import('java.lang.System');
    javaLangSystem.out.printlnSync('Hello World');
    ```

    검증 완료

2. 단순한 Java project를 jar로 만든 후 node로 테스트 해보기
    - 문제1: Eclipse에서 jar파일로 단순히 변환을 하게 되면 mainfest가 특정되지 않는다.
    - Solution:  
    Export → java → Runnable JAR file → Launch configuration에서 클래스 선택하면됨. → Export destination : simpletest\simple4.jar (browser 버튼 눌러서 내가 만들고 싶은 파일명적고 저장 누르면 됨

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b3d361d2-cc8f-4f56-8bc0-1403fcb60314/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b3d361d2-cc8f-4f56-8bc0-1403fcb60314/Untitled.png)

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3ffa54bf-3258-4c7b-b36c-769d9a390ec6/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3ffa54bf-3258-4c7b-b36c-769d9a390ec6/Untitled.png)

    - 문제 2: class 파일 이름이 제대로 안뜨는 문제 발생

    ```bash
    var clazz = java.findClassSync(name); // TODO: change to Class.forName when classloader issue is resolved.
                       ^

    Error: Could not create class MyClass2
    java.lang.NoClassDefFoundError: MyClass2
    Caused by: java.lang.ClassNotFoundException: MyClass2
            at java.net.URLClassLoader.findClass(URLClassLoader.java:382)
            at java.lang.ClassLoader.loadClass(ClassLoader.java:418)
            at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:355)
            at java.lang.ClassLoader.loadClass(ClassLoader.java:351)

        at Java.java.import (D:\boba-node-java-api-test2\node_modules\java\lib\nodeJavaBridge.js:227:20)
        at Object.<anonymous> (D:\boba-node-java-api-test2\javaJarModuleTest.js:4:26)
        at Module._compile (internal/modules/cjs/loader.js:776:30)
        at Object.Module._extensions..js (internal/modules/cjs/loader.js:787:10)
        at Module.load (internal/modules/cjs/loader.js:653:32)
        at tryModuleLoad (internal/modules/cjs/loader.js:593:12)
        at Function.Module._load (internal/modules/cjs/loader.js:585:3)
        at Function.Module.runMain (internal/modules/cjs/loader.js:829:12)
        at startup (internal/bootstrap/node.js:283:19)
        at bootstrapNodeJSCore (internal/bootstrap/node.js:622:3)

    ```

    - Solution:

    ```markup
    java.classpath.push('D:\boba-node-java-api-test2\simple.jar');
    ->java.classpath.push('D:\\boba-node-java-api-test2\\simple.jar');
    ```

    검증 완료

    - Java 코드

    ```java
    public class MyClass {
     public static void main(String[] args){
    	 System.out.println("Now the output is redirected!");
     }
     public static int addNumbers(int a, int b) {
    	 return a + b;
     	}
     }
    ```

    - node.js 코드

    ```jsx
    var java = require('java');
    java.classpath.push(".");
    java.classpath.push('D:\\boba-node-java-api-test2\\simple.jar');
    var MyClass = java.import("MyClass");// 클래스 이름
    var result = MyClass.addNumbersSync(1, 2);//메소드 이름에 sync 붙이기
    console.log(result);
    ```

    - 주의 사항
        - 경로를 쓸때  \\ 이거로 쓰기!
3. 단순한 Spring Boot 프로젝트를 jar로 만든 후 node로 테스트 해보기
    - 문제1 :Spring Boot 프로젝트가 jar file로 안만들어짐
    - solution :

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bed74ebb-e725-4704-abc6-b599ff7128e5/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bed74ebb-e725-4704-abc6-b599ff7128e5/Untitled.png)

    계속 Build가 실패했던 이유는 gradle build 로 사용해서.... 
    eclipse..... 자료가 거지같다.... 인텔리제이로 할걸....
## Reference

https://github.com/joeferner/node-java
