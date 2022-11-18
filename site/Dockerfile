FROM node:16.15.0-alpine

# env
ENV HOST 0.0.0.0
ENV APP_HOME /code/bbs-go/site

# code
WORKDIR "$APP_HOME"
ADD .npmrc ./
RUN npm install -g pnpm@7.16.1 --registry=https://registry.npmmirror.com
ADD package.json ./
ADD pnpm-lock.yaml ./
RUN pnpm fetch

COPY . ./
COPY nuxt.config.docker.js ./nuxt.config.js

RUN pnpm install --offline

# build
ENV NODE_ENV production
RUN pnpm run build

EXPOSE 3000
CMD ["npm","start"]
