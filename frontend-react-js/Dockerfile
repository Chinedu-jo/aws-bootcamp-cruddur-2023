FROM node:16.19-alpine

WORKDIR /frontend-react-js

COPY . /frontend-react-js

RUN npm install

RUN apk --update --no-cache add curl

ENV PORT=3000

EXPOSE ${PORT}

CMD ["npm", "start"]