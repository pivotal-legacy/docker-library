# Inspired by https://github.com/meAmidos/dcind/
FROM node:7.2.0

# Install elm
RUN npm install -g elm-test@0.18.0
RUN npm install -g elm@0.18.0

ENV DOCKER_VERSION=1.12.1 \
    DOCKER_COMPOSE_VERSION=1.8

# Install Docker and Docker Compose
RUN apt-get update
RUN apt-get -y install make curl python-dev python-pip iptables
RUN curl https://get.docker.com/builds/Linux/x86_64/docker-${DOCKER_VERSION}.tgz | tar zx
RUN mv /docker/* /bin/ && chmod +x /bin/docker*
RUN pip install docker-compose==${DOCKER_COMPOSE_VERSION}

# Install entrykit
RUN curl -L https://github.com/progrium/entrykit/releases/download/v0.4.0/entrykit_0.4.0_Linux_x86_64.tgz | tar zx && \
    chmod +x entrykit && \
    mv entrykit /bin/entrykit && \
    entrykit --symlink

# Include useful functions to start/stop docker daemon in garden-runc containers in Concourse CI.
# Example: source /docker-lib.sh && start_docker
COPY docker-lib.sh /docker-lib.sh

ENTRYPOINT [ \
	"switch", \
		"shell=/bin/sh", "--", \
	"codep", \
		"/bin/docker daemon" \
]