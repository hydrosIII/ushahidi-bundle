FROM node:6.11.4-alpine

RUN apk update && \
 apk upgrade && \
  apk add nginx  git bash && \
 ln -sf /dev/stdout /var/log/nginx/access.log && \
 ln -sf /dev/stderr /var/log/nginx/error.log

RUN mkdir -p /run/nginx
COPY nginx-site.conf /etc/nginx/sites-available/default



RUN git clone https://github.com/ushahidi/platform-client \
      /tmp/client && \
  cd /tmp/client && \
  git checkout $VERSION && \
  git checkout -b deploy 
 


RUN cd /tmp/client && \
  npm install && \
  npm install gulp && \
  node_modules/gulp/bin/gulp.js build && \
  rm -r /var/www/* && \
  mv /tmp/client/server/www /var && \
  npm cache clean && \
  rm -rf /tmp/*

COPY start.sh /

EXPOSE 80

CMD ["/start.sh"]
