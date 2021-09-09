FROM mysql:5.6

WORKDIR /var/www

ENV USER=denisl
ENV UID=1000
ENV GID=1000

RUN groupadd -g "$GID" "$USER"
RUN useradd -u "$UID" -ms /bin/bash -g "$USER" "$USER"

COPY . /var/www
COPY --chown="$USER":"$USER" . /var/www
USER "$USER"