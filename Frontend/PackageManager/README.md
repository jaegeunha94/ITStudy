# NPM의 등장
* 여러 버전의 동일한 패키지를 한 프로젝트에서 사용할 수 있게 하자
* 설치 방식을 통일하자
* 패키지 관련 정보가 들어있는 메타 데이터를 간소화하자
* 누구나 배포할 수 있게 하자


# Yarn의 등장(Yet Another Resource Negotiator)
* 병렬화를 통한 속도 개선
* 자동화 된 lock 생성
* 의존된 트리 알고리즘 변경
* 캐시사용


# NPM 계열의 한계
## 1. 비 효율적 의존성 검색

![npm](./npm.PNG)

* 패키지를 부모 디렉토리부터 시작해서 루트 디렉토리까지 패키지를 찾아나가는 원리
* 패키지가 없다면 부모 디렉토리까지 계쏙 검색 비효율적
* 잘못된 의존성을 가진 것을 사용할 수 있는 위험성이 있음

## 2. 유령 의존성(호이스팅)

![npm](./hoist.PNG)

* 평탄화 과정에서 사용하지 않는 모듈이 호이스팅 되서 원하지 않는것을 다운될 수 있다.


## 3. 너무 무거운 node_modules


# Yarn v2의 틍장
1. Zero Install(yarn install을 불필요)

2. 오프라인시에 캐싱기능으로 사용 가능

3. CI 배포시에 clone만 하면 의존성이 다 있기 떄문에 배포 속도를 줄일 수 있다.

4. Plug'n'Play

![yarnv2](./yarnv2.PNG)


# 📌yarn, npm 차이

## 1. Parallel installation of packages, packages 병렬 설치

* NPM은 여러 패키지를 설치할 때, 패키지 별로 순차적으로 실행
* YARN은 이러한 작업을 병렬로 설치하므로 퍼포먼스와 속도가 증가

## 2. Automatic Lock file generation, 자동 lock 파일 생성

* NPM과 YARN 모두 패키지에서 프로젝트의 종속성과 버전 번호를 추적
* json 파일 종속성을 설치할 때마다 종속성 버전이 버전 번호 앞에 ^로 시작될 수 있음
* 최신 버전이 있는 경우 패키지 파일에 언급된 버전이 아닌 자동으로 설치
* 패키지를 자동으로 변경하지 않기 위한 두 가지 방법
* 하나는 잠금 파일을 생성
* 패키지 파일에 ^을 제거
* 종속성이 추가되면 YARN은 yarn.lock 파일을 자동으로 추가, 항상 업데이트
* NPM은 npm-shrinkwrap.json이 존재하는 경우에만 업데이트


## 3. 보안성
* NPM은 다른 패키지를 즉시 포함시킬 수 있는 코드를 자동으로 실행하므로, 보안 시스템에 여러 가지 취약성이 발생한다.  
* 반면에, YARN은 yarn.lock 또는 package.json 파일에 있는 파일만 설치한다.  
* 따라서 YARN이 NPM 패키지보다 보안이 강화된 것으로 간주된다.



# 참고
[테코톡 비녀의 Package Manager](https://www.youtube.com/watch?v=Ds7EjE8Rhjs)
