# Frontend or Fullstack project strucuture.

- CRA (ReactJS)
- NestJS
- typescript

## Frontend
- `PORT` is 3000
- `{"proxy": "http://localhost:8080"}`
- `source code` is `./ui`
- `build output` is `./static`
- REST API 호출시에는 반드시, `/` 로 하는 로컬서버 사용할 것.
- 외부 서버의 API 리소스를 사용할 때도, 로컬서버에서 외부서버 API 사용후, 데이터 리턴 받을 것.

## Backend
- `PORT` is 8080
- `source code` is `./src`
- `build output` is `./dist`

## Coding

개발 할 때는, 서버를 둘다 띄움.

```bash
# frontend (CRA)
yarn --cwd ui start
```

```bash
# backend (NextJS)
yarn start # dev
```

## BUILD

- UI project
- Backend project

### Build UI project 

아래 명령어
```bash
yarn --cwd ui build
```

### Build Backend project

```bash
yarn build
```


## Deployment
`./static` 폴더와 `./dist` 폴더를 도커 파일로 복사하거나, 같이 배포할 것.

```Dockerfile
# 기본 Node image를 통하여 현재 NestJs Project를 Build
FROM node:16.19.1 AS builder
# WORKDIR을 설정
WORKDIR /app
# 현재 프로젝트를 WORKDIR에 복사 이 때 dockerignore에 추가되어 있는 것들은 복사되지 않는다.
COPY . .
# 의존성 추가
RUN yarn
# 빌드
RUN yarn build

# alpine을 이용하여 도코 이미지 경량화
FROM node:16.19.1-alpine
# WORKDIR을 설정
WORKDIR /app
# NODE_ENV production으로 설정
ENV NODE_ENV production
# builder에서 build 된 결과물
COPY --from=builder /app ./
CMD ["yarn","start:prod"]

```
