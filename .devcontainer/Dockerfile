FROM alpine:3.16

ENV TZ UTC
SHELL ["/bin/ash", "-eo", "pipefail", "-c"]
RUN apk add --no-cache \
	apache2 php-apache2 \
	php php-curl php-gmp php-intl php-mbstring php-xml php-zip \
	php-ctype php-dom php-fileinfo php-iconv php-json php-opcache php-openssl php-phar php-session php-simplexml php-xmlreader php-xmlwriter php-xml php-tokenizer php-zlib \
	php-pdo_sqlite php-pdo_mysql php-pdo_pgsql \
	bash composer curl docker-cli-buildx git gpg make nodejs npm shellcheck shfmt sudo

RUN rm -f /etc/apache2/conf.d/languages.conf /etc/apache2/conf.d/info.conf \
		/etc/apache2/conf.d/status.conf /etc/apache2/conf.d/userdir.conf && \
	sed -r -i "/^\s*LoadModule .*mod_(alias|autoindex|negotiation|status).so$/s/^/#/" \
		/etc/apache2/httpd.conf && \
	sed -r -i "/^\s*#\s*LoadModule .*mod_(deflate|expires|headers|mime|remoteip|setenvif).so$/s/^\s*#//" \
		/etc/apache2/httpd.conf && \
	sed -r -i "/^\s*(CustomLog|ErrorLog|Listen) /s/^/#/" \
		/etc/apache2/httpd.conf

RUN addgroup developer && \
	adduser --ingroup developer --disabled-password developer && \
	echo "developer ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/developer

ENV COPY_LOG_TO_SYSLOG On
ENV COPY_SYSLOG_TO_STDERR On
ENV CRON_MIN ''
ENV FRESHRSS_ENV 'development'
ENV LISTEN '0.0.0.0:8080'

EXPOSE 8080