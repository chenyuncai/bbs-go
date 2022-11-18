FROM node:18.12.1-slim as builder

RUN mkdir -p /app
WORKDIR /app
ADD .npmrc /app/
RUN npm install -g pnpm@7.16.1 --registry=https://registry.npmmirror.com
RUN pnpm -v
ADD package.json /app/
ADD pnpm-lock.yaml /app/
RUN pnpm fetch

# 将整个项目复制下来
COPY . /app/
RUN pnpm install --offline

RUN pnpm run build

FROM nginx:1.21.6-alpine
LABEL maintainer="Ray Chen<chenyuncai@tmxmall.com>"

# 设置docker容器运行时区
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN echo 'Asia/Shanghai' > /etc/timezone

RUN mkdir -p /nginx/webroot/
COPY --from=builder /app/dist /nginx/webroot/admin
ADD nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

STOPSIGNAL SIGTERM

CMD ["nginx", "-g", "daemon off;"]
