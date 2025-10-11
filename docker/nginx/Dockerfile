# install inginx image
FROM nginx:1.25-alpine

# copy nginx config from host to container
# harus di-copy karena sudah tidak bisa melakukan amount dalam server
COPY default.conf /etc/nginx/conf.d/default.conf