FROM python:3.7-slim-buster

# Software version management
ENV NGINX_VERSION=1.16.1-3
ENV SUPERVISOR_VERSION=4.1.0
ENV GUNICORN_VERSION=20.0.4
ENV GEVENT_VERSION=1.5.0

# Environment setting
ENV APP_ENVIRONMENT production

# Flask demo application
COPY ./app /app
RUN pip install -r /app/requirements.txt

# System packages installation
RUN apt-get update && apt-get install -y \ 
    nginx \ 
    supervisor \
    && rm -rf /var/lib/apt/lists/*

# Nginx configuration
RUN echo "daemon off;" >> /etc/nginx/nginx.conf
RUN rm /etc/nginx/sites-enabled/default
COPY nginx.conf /etc/nginx/conf.d/nginx.conf

# Supervisor configuration
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
COPY gunicorn.conf /etc/supervisor/conf.d/gunicorn.conf

# Gunicorn installation
RUN pip install gunicorn==$GUNICORN_VERSION gevent==$GEVENT_VERSION

# Gunicorn default configuration
COPY gunicorn.config.py /app/gunicorn.config.py

WORKDIR /app

EXPOSE 80

CMD ["/usr/bin/supervisord"]
