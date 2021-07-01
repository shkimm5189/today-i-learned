## 개념


#### 이미지
- 운영체제와 소프트웨어를 담고있는 컨테이너 실행 이전의 상태.
- 각 이미지는 '리포지터리:태그'로 식별된다.
> 태그란? imagename:tag형태로 되어있는데 통상적으로 tag는 Architect나 버전을 의미한다.
```
컨테이너이미지에는 절대 커널이 포함될수없다.
하지만 컨테이너 이미지에도 OS 이미지가 있다(Centos, Fedora, ubuntu ...)
OS image는 base image라고도 하고 다른 이미지를 생성하기 위한 용도로 사용한다.
lib, Binary만 포함 시킨다.
```
#### 리포지토리 (레지스트리)
- 이미지 보관소
- 리포지토리의 이름에 버전 등을 의미하는 태그를 붙여 각각의 이미지를 구별하여 보관 가능
- 태그를 생략하면 최신을 의미하는 latest가 사용
#### docker hello-world 실행 순서
1. 도커 클라이언트가 도커 데몬접속
2. 도커 데몬이 도커 허브에서 이미지를 pull함
3. 도커 데몬에서 새 컨테이너 생성후 해당 서비스의 결과를 실행
4. 도커 데몬은 해당 출력을 도커 클라이언트의 표준 출력으로 전송

#### 상태
- 이미지 : 컨테이너의 모형이 되는 것으로 실행되기 전의 상태
- 실행 : 컨테이너 위에서 프로세스가 실행중인 상태
- 정지 : 프로세스의 종료 코드, 로그가 보존 된채 정지한 상태

![도커_라이플사이클](images/dockerLifeCycle.png)


### docker command


## 컨테이너 개발
### 이미지 빌드 흐름
```
1. 디렉토리를 준비하여 이미지에 포함 시킬 파일 모음
2. Dockerfile 작성
3. Container에서 실행할 앱 코드 작성, 유닛 테스트 실행
4. 이미지 빌드
5. 컨테이너 실행 후 동작 확인
```
#### Dockerfile 작성 모범 사례
[도커 레퍼런스](https://docs.docker.com/engine/reference/builder/)<br>
[도커파일 모범사례](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
