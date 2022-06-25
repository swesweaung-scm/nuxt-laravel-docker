FROM node:16-alpine

# create destination directory
RUN mkdir -p /var/www
WORKDIR /var/www

# update and install dependency
RUN apk update && apk upgrade
RUN apk add git

# copy the app, note .dockerignore
# COPY . /var/www/
# RUN yarn install
# RUN yarn build

EXPOSE 8090

ENV NUXT_HOST=0.0.0.0
ENV NUXT_PORT=8090

CMD ["yarn", "dev"]