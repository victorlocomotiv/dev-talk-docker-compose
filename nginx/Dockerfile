FROM nginx:1.15.0

RUN apt-get update \
	&& apt-get install wget git gcc build-essential libpcre3 libpcre3-dev zlib1g zlib1g-dev -y

# Instalando e compilando Módulo, em modo dinâmico, do NGINX.
RUN mkdir -p /var/nginx-module/ \
	&& wget -P /var/nginx-module/ http://nginx.org/download/nginx-1.15.0.tar.gz \
	&& tar -xzvf /var/nginx-module/nginx-1.15.0.tar.gz -C /var/nginx-module/
RUN git clone git://github.com/yaoweibin/ngx_http_substitutions_filter_module.git /var/nginx-module/ngx_http_substitutions_filter_module
COPY ./modules/ngx_http_substitutions_filter_module /var/nginx-module/ngx_http_substitutions_filter_module/
RUN cd /var/nginx-module/nginx-1.15.0/ \
	&& ./configure --with-compat --add-dynamic-module=../ngx_http_substitutions_filter_module \
	&& make modules \
	&& cp /var/nginx-module/nginx-1.15.0/objs/ngx_http_subs_filter_module.so /etc/nginx/modules/

# Certificates
RUN mkdir -p /etc/nginx/certificates
COPY certificates /etc/nginx/certificates
RUN chmod 755 -Rf /etc/nginx/certificates/

# Config files
COPY nginx.conf /etc/nginx/nginx.conf
RUN rm /etc/nginx/conf.d/*
COPY .htpasswd /etc/nginx/

EXPOSE 80
EXPOSE 443