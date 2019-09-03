FROM php5.6-fpm-basic:latest

RUN apt-get install -y mongodb-clients

#----------------------------------------------------------
# NodeJS and Dependecies
#----------------------------------------------------------

RUN curl -sL https://deb.nodesource.com/setup_10.x -o nodesource_setup.sh \
    && bash nodesource_setup.sh \
    && apt-get install -y nodejs \
    && npm install -g grunt-cli \
    && npm install -g bower \
    && touch /root/.bowerrc \
    && echo '{"allow_root": true}' > /root/.bowerrc


#----------------------------------------------------------
# php exts
#----------------------------------------------------------

RUN yes | pecl install mongo \
    && docker-php-ext-enable mongo \
    && no | pecl install redis \
    && docker-php-ext-enable redis

ARG UID
ARG GID
ARG USER

RUN groupadd -r -g $GID $USER \
    && useradd -r -u $UID -g $USER $USER \
    && adduser $USER sudo \
    && mkdir /home/$USER \
    && chown -R $UID:$GID /home/$USER \
    && echo "$USER ALL=(ALL) NOPASSWD: ALL">> /etc/sudoers


#----------------------------------------------------------
# Download App
#----------------------------------------------------------
ARG GITLAB_TOKEN

RUN git config --global url.https://oauth2:$GITLAB_TOKEN@git.sirian.org/.insteadOf git@git.sirian.org: \
    && mkdir -p /var/www/mecash/ \
    && cd /var/www/mecash \
    && git clone git@git.sirian.org:mecash/mecash.git . \
    && git config core.filemode false

#----------------------------------------------------------
# Builds App
#----------------------------------------------------------

RUN cd /var/www/mecash \
    && composer install \
    && npm install

#----------------------------------------------------------
# Copy GeoIP
#----------------------------------------------------------

RUN cp /var/www/mecash/node_modules/maxmind/test/data/test-data/GeoIP2-City-Test.mmdb /var/www/mecash/var/GeoIP2-City.mmdb
#----------------------------------------------------------



