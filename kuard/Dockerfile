# STAGE 1: Build
FROM golang:1.12-alpine AS build

RUN apk update && apk upgrade && apk add --no-cache git nodejs bash npm make dos2unix

RUN go get -u github.com/jteeuwen/go-bindata/...

WORKDIR /go/src/github.com/kubernetes-up-and-running/kuard

COPY . .

ENV VERBOSE=0
ENV PKG=github.com/kubernetes-up-and-running/kuard
ENV ARCH=amd64
ENV VERSION=test

RUN dos2unix build/build.sh
RUN chmod +x build/build.sh
RUN build/build.sh

# STAGE 2: Runtime
FROM alpine

USER nobody:nobody
COPY --from=build /go/bin/kuard /kuard

CMD [ "/kuard" ]
