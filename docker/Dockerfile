FROM node:8-alpine

RUN apk add --update --no-cache git \
                                libzmq \
                                zeromq-dev \
                                python \
                                make \
                                g++

WORKDIR /insight

# Copy xazabcore-node
RUN git clone --branch master --single-branch --depth 1 https://github.com/xazab/xazabcore-node.git .

# Copy config file
COPY xazabcore-node.json .

# Install NPM modules
RUN npm ci

ARG VERSION
ARG MAJOR_VERSION

# Install Insight modules
RUN bin/xazabcore-node install @xazab/insight-api@${MAJOR_VERSION}
RUN bin/xazabcore-node install @xazab/insight-ui@${VERSION}

FROM node:8-alpine

LABEL maintainer="Xazab Developers <dev@xazab.org>"
LABEL description="Dockerised Insight-Xazab"

WORKDIR /insight

# Copy project files
COPY --from=0 /insight/ .

EXPOSE 3001

CMD ["bin/xazabcore-node", "start"]
