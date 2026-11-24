FROM node:lts-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 9000
ENV MONGO_URI=""
CMD ["npm", "start"]