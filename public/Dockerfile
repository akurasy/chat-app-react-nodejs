FROM node:slim AS builder

WORKDIR /public

COPY . /public

RUN npm install

ENV REACT_APP_LOCALHOST_KEY="chat-app-current-user"

RUN npm run build

EXPOSE 3000

ENTRYPOINT [ "npm", "start" ]
