ARG WORDPRESS_VERSION
FROM wordpress:$WORDPRESS_VERSION

WORKDIR /var/www/html/

RUN echo "define( 'WP_DEBUG_LOG', getenv_docker('WORDPRESS_DEBUG_LOG', '') );" >> ./wp-config-docker.php && \
    echo "define( 'WP_DEBUG_DISPLAY', getenv_docker('WORDPRESS_DEBUG_DISPLAY', '') );" >> ./wp-config-docker.php
