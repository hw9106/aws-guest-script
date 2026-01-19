# Node.js 환경에서 React 빌드
FROM node:20 AS build
# 컨테이너 내부 작업 폴더를 /app으로 설정 (자동 생성됨)
WORKDIR /app

# package.json과 package-lock.json을 먼저 복사하여 의존성 설치
COPY package.json package-lock.json* ./
RUN npm install --silent

# 현재 폴더(호스트)의 모든 파일을 컨테이너의 /app으로 복사
COPY . .
RUN npm run build

# 2단계: Nginx로 정적 파일 서비스
FROM nginx:stable-alpine

# React 앱 빌드 결과물 복사
COPY --from=build /app/build /usr/share/nginx/html

# optional: custom nginx config for SPA routing
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
