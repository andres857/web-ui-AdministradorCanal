FROM node:14.15-alpine3.12

WORKDIR /usr/src/app/my-app

COPY package*.json ./
COPY vue.config.js ./

RUN npm install

EXPOSE 8080

CMD ["npm", "run", "serve"]