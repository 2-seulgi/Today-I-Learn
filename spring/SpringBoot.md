# Spring Boot

## Gradle

Gradle은 Groovy 언어를 기반으로 한 빌드 자동화 도구, Maven이나 Ant 등의 단점을 보완하기 위해 만들어짐

### 특징

Gradle Wrapper를 사용하여 Gradle이 설치되지 않은 시스템에서도 프로젝트를 빌드할 수 있다(환경에 종속되지 않음). 또한 Maven의 중앙 저장소를 지원하므로 라이브러리를 그대로 사용할 수 있다.

### 빌드 단계

1. 초기화
   프로젝트 구조를 이해하는 단계로, 이 단계에서는 `settings.gradle` 파일이 호출되어 실행된다.
   프로젝트 내의 어떤 하위 디렉토리에서도 gradle을 실행할 수 있다. gradle이 없는 디렉토리에서 실행할 경우, 상위 디렉토리에서 `settings.gradle`을 찾아 실행하고 프로젝트 구성에 따라 멀티 모듈 또는 싱글 모듈 프로젝트로 빌드한다.

2. 구성
   `build.gradle` 파일이 실행되는 단계. 빌드 스크립트 파일은 프로젝트를 빌드하기 위해 의존성, 플러그인 설정 등을 정의한다.

3. 실행
   의존성 순서에 따라 작업을 실행한다. 주로 라이브러리 다운로드, 코드 컴파일, 입력 읽기, 출력 출력 등을 포함한다.

### 추가 사항

- **Incremental Build**: Gradle은 증분 빌드를 지원하여 이미 수행된 작업은 다시 수행하지 않음으로써 빌드 시간을 단축할 수 있다.
- **다양한 언어 지원**: Gradle은 Java뿐만 아니라 C/C++, Python 등 다양한 언어를 지원하며 이를 통해 더 광범위한 프로젝트에 활용될 수 있다.
- **플러그인 및 확장성**: 다양한 플러그인을 통해 기능을 확장할 수 있으며, 사용자 정의 플러그인을 생성하여 Gradle 빌드 프로세스를 사용자화한다.
