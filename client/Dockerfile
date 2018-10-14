# if you're doing anything beyond your local machine, please pin this to a specific version at https://hub.docker.com/_/node/
FROM node:10

RUN mkdir -p /opt/app

# set our node environment, either development or production
# defaults to development, compose overrides this to development on build and run
ARG NODE_ENV=development
ENV NODE_ENV $NODE_ENV

# default to port 80 for node, and 9229 and 9230 (tests) for debug
ARG PORT=3000
ENV PORT $PORT
EXPOSE $PORT 9229 9230

# you'll likely want the latest npm, reguardless of node version, for speed and fixes
RUN npm i yarn -g

# install dependencies first, in a different location for easier app bind mounting for local development
WORKDIR /opt
COPY package.json ./
COPY yarn.lock ./
RUN yarn install
ENV PATH /opt/node_modules/.bin:$PATH


# copy in our source code last, as it changes the most
WORKDIR /opt/app
COPY . /opt/app

# if you want to use npm start instead, then use `docker run --init in production`
# so that signals are passed properly. Note the code in index.js is needed to catch Docker signals
# using node here is still more graceful stopping then npm with --init afaik
# I still can't come up with a good production way to run with npm and graceful shutdown
CMD [ "yarn", "start" ]
