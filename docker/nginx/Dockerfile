FROM nginx:1.17

ARG LARAVEL_ROOT=/usr/share/nginx/html

COPY default.conf /etc/nginx/conf.d/
RUN sed "s@root /usr/share/nginx/html/public@root ${LARAVEL_ROOT}/public@" -i /etc/nginx/conf.d/default.conf

ARG USER=nginx
ARG GROUP=nginx

RUN sed "/^user/d" -i /etc/nginx/nginx.conf
RUN echo "user  ${USER} ${GROUP};" >> /etc/nginx/nginx.conf

RUN mkdir /etc/cron.d
RUN echo '* * * * * cd ${LARAVEL_ROOT}/public && php artisan schedule:run >> /dev/null 2>&1' > /etc/cron.d/laravel
RUN chmod 644 /etc/cron.d/laravel
RUN touch /var/log/cron.log
