FROM wodby/php:7.0
MAINTAINER brilsergei <brilsergei@gmail.com>
MAINTAINER Wodby <admin@wodby.com>
ARG HOST_UID=1000
ARG HOST_GID=1000

    # Install Drupal Console
RUN curl https://drupalconsole.com/installer -o /usr/local/bin/drupal && \
    chmod +x /usr/local/bin/drupal && \

    # Install drush
    su-exec www-data composer global require drush/drush && \

    su-exec www-data composer clear-cache

    # Create group and user with same GID and UID as on host machine and assign it www-data group
RUN addgroup -g $HOST_GID host-group && \
	adduser -u $HOST_UID -D -G host-group host-user && \
	adduser host-user www-data && \

	# Change the site root owner
	chown -R $HOST_UID:$HOST_GID /var/www

	# Initialize console for host-user
USER host-user
RUN composer global require hirak/prestissimo:^0.3 --optimize-autoloader && \
    rm -rf ~/.composer/.cache

USER root
RUN drupal self-update
