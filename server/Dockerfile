FROM node:slim

WORKDIR /server

COPY . /server

RUN npm install

ENV PORT=5000

ENV MONGO_URL="mongodb://mongo:27017/chat"

EXPOSE 5000

ENTRYPOINT [ "npm", "start" ]



