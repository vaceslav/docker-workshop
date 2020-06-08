### STAGE 1: Build ###

# We label our stage as 'builder'
FROM node:12-alpine as angular-builder

COPY package.json yarn.lock angular.json ./


## Storing node modules on a separate layer will prevent unnecessary npm installs at each build
RUN yarn && mkdir /ng-app && cp -R ./node_modules ./ng-app

WORKDIR /ng-app

COPY . .

## Build the angular app in production mode and store the artifacts in dist folder
#RUN $(npm bin)/ng build --prod --build-optimizer
RUN  yarn build --prod --build-optimizer



# Stage 2
# Stage 1, based on Nginx, to have only the compiled app, ready for production with Nginx
FROM nginx:1.13.3-alpine

## Remove default nginx website
RUN rm -rf /usr/share/nginx/html/*

## From 'builder' stage copy over the artifacts in dist folder to default nginx public folder
COPY --from=angular-builder /ng-app/dist/testApp /usr/share/nginx/html

CMD ["nginx", "-g", "daemon off;"]