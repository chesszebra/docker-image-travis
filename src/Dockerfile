FROM ruby:latest

ENV GOSU_VERSION 1.10

# Create a custom user to run the container on
RUN useradd travis \
 && mkdir -p /home/travis \
 && touch /home/travis/.profile \
 && chown -R travis:travis /home/travis \
 && mkdir /data \
 && chown -R travis:travis /data

# Copy over the entry point so we can modify the travis user
COPY usr/bin/entry-point /usr/bin/entry-point
RUN chmod +x /usr/bin/entry-point

# Install Travis
RUN gem install travis --no-rdoc --no-ri

# The volume that can be mounted
VOLUME /data

# The directory to work from
WORKDIR /data

# Install gosu
RUN set -ex; \
	\
	fetchDeps=' \
		ca-certificates \
		wget \
	'; \
	apt-get update; \
	apt-get install -y --no-install-recommends $fetchDeps; \
	rm -rf /var/lib/apt/lists/*; \
	\
	dpkgArch="$(dpkg --print-architecture | awk -F- '{ print $NF }')"; \
	wget -O /usr/local/bin/gosu "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$dpkgArch"; \
	wget -O /usr/local/bin/gosu.asc "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$dpkgArch.asc"; \
	\
# verify the signature
	export GNUPGHOME="$(mktemp -d)"; \
	gpg --keyserver ha.pool.sks-keyservers.net --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4; \
	gpg --batch --verify /usr/local/bin/gosu.asc /usr/local/bin/gosu; \
	rm -r "$GNUPGHOME" /usr/local/bin/gosu.asc; \
	\
	chmod +x /usr/local/bin/gosu; \
# verify that the binary works
	gosu nobody true; \
	\
	apt-get purge -y --auto-remove $fetchDeps

# Always run travis
ENTRYPOINT ["/usr/bin/entry-point", "travis"]
