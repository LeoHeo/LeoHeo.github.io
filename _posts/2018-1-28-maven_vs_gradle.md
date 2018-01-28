---
layout: post
title: maven vs gradle
---

# gradle이 maven 보다 좋은점

## 빠르다 
- [공식 설명으로는 100배](https://docs.gradle.org/current/userguide/tutorial_java_projects.html)
- [Gradle은 정말 Maven보다 100배나 빠를까?
](https://www.holaxprogramming.com/2017/07/04/devops-gradle-is-faster-than-maven/)

## 설치없이 build가 가능하다.
- `$ mvn package` <-- maven은 build를 할려면 꼭 설치가 되어있어야 한다.
- `$ ./gradlew build` <-- gradle project일 경우 gradle 설치 없이 빌드 할 수 있다.

# jar 파일 실행방법
`$ java -jar {build.jar}` <-- jar 파일로 나온걸 이 커맨드로 실행 할 수 있다. 