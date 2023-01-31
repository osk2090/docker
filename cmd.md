### 1. 먼저 프로젝트 디렉토리에서 Dockerfile을 생성한다.
### 2. 그 안에 docker 명령어를 입력한다.

FROM node -> 베이스가되는 이미지 선언

WORKDIR /app -> 작업하는 기본 디렉토리를 docker에 선언

COPY . /app -> .(로컬에 있는 모든 파일) 복사하여 docker의 기본 디렉토리에 추가

RUN npm install -> npm 의존성 설치

EXPOSE 80 -> 외부와 연결할 포트를 열어둠(아직 외부와 연결이 안됨)

CMD ["node","server.js"] -> RUN이 아닌 CMD를 이용하여 이미지 생성시 서버시작이 아닌 컨테이너가 생성되면 서버 시작
