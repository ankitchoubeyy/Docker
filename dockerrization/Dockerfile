# Pull base image
FROM node:16

# Create app directory
WORKDIR /Dockerrization

# Install app dependencies
COPY package*.json ./

RUN npm install

# Bundle app source
COPY . .

EXPOSE 3000
CMD [ "node", "index.js" ]

