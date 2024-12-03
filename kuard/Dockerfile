# STAGE 1: Build
FROM golang:1.21-alpine AS build  

# Install Node, NPM, ve gerekli araçlar
RUN apk update && apk upgrade && apk add --no-cache \
    git \
    nodejs \
    bash \
    npm \
    dos2unix  # dos2unix, Windows tarzı satır sonlarını temizlemek için gerekli

# Get dependencies for Go part of build
RUN go get -u github.com/jteeuwen/go-bindata/...

WORKDIR /go/src/github.com/kubernetes-up-and-running/kuard

# Copy all sources in
COPY . .

# This is a set of variables that the build script expects
ENV VERBOSE=0
ENV PKG=github.com/kubernetes-up-and-running/kuard
ENV ARCH=amd64
ENV VERSION=test

# When running on Windows 10, you need to clean up the ^Ms in the script
RUN dos2unix build/build.sh

# Do the build. Script is part of incoming sources.
RUN chmod +x build/build.sh
RUN build/build.sh

# STAGE 2: Runtime
FROM alpine:latest 

# Install runtime dependencies
RUN apk update && apk add --no-cache ca-certificates

USER nobody:nobody
COPY --from=build /go/bin/kuard /kuard

CMD [ "/kuard" ]
