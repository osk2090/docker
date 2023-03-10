### 1. 먼저 프로젝트 디렉토리에서 Dockerfile을 생성한다.
### 2. 그 안에 docker 명령어를 입력한다.

FROM node -> 베이스가되는 이미지 선언

WORKDIR /app -> 작업하는 기본 디렉토리를 docker에 선언

COPY . /app -> .(로컬에 있는 모든 파일) 복사하여 docker의 기본 디렉토리에 추가

RUN npm install -> npm 의존성 설치

EXPOSE 80 -> 외부와 연결할 포트를 열어둠(아직 외부와 연결이 안됨)

CMD ["node","server.js"] -> RUN이 아닌 CMD를 이용하여 이미지 생성시 서버시작이 아닌 컨테이너가 생성되면 서버 시작

### 3. 로컬의 터미널을 이용하여 'docker build .' 로 도커 이미지 생성
### 4. 이미지 생성후 마지막에 이미지의 코드명을 이용하여 'docker run (이미지 코드명)'
### 5. 하지만 여기서 도커는 아직 독립된 공간에 있으며 내부와 외부 포트가 연결되어 있지 않기 때문에 'docker run -p (연결할 외부포트번호):(docker안에서 선언된 포트번호) (이미지 코드명)'으로 build한다.

## 수정된 코드로 새로운 이미지 생성
도커의 이미지는 독립적이기 때문에 코드가 수정되면 새로운 이미지를 생성하여 컨테이너로 운영된다.
Dockerfile이 있는 디렉토리에서 docker build . 로 새로운 이미지를 생성한다.
그리고 컨테이너 동작시 새로운 컨테이너가 생성되면서 수정된 코드가 적용됨을 확인할 수 있다.

도커는 레이어 기반의 아키텍처이다.
예시로 기존 이미지에 파일 하나 또는 소스가 수정되었다고 치자.
그럼 다시 이미지를 생성하면 처음부터 수백메가의 데이터를 다시 다운받아야한다.
그래서 레이어 구조는 이를 최적화하고자 한다.

COPY . /app

RUN npm install

복사하는 부분에서 변경이 일어났다는것을 인지하면 그 뒤에 동작하는 명령어들은 새로 시작하게 된다. 
기존의 Dockerfile 소스에서 해당 로직대로 한다면 계속 npm install을 할것이다. 

COPY package.json /app

RUN npm install

COPY . /app

먼저 package.json을 복사하여 npm install이 일어나지 않도록 해준다.
그리고 수정된 소스부터 다시 이미지 생성이 되도록 해줘야지 최적화를 해줄수 있다.

attached & detached의 차이
docker run은 디폴트로 attached 상태이며 현재 컨테이너에 연결되어 출력되는 로그들을 볼수 있는 상태
docker start는 디폴트로 detached 상태이며 현재 컨테이너와 분리된 상태

만약 run상태에서 detached 모드로 설정을 원한다면 -a 옵션 추가
만약 start상태에서 attached 모드로 설정을 원한다면 docker logs 컨테이너id 입력