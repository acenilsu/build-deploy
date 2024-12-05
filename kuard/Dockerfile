# STAGE 1: Build
FROM golang:1.20-alpine AS build

# Gerekli bağımlılıkları kur
RUN apk update && apk add --no-cache git bash npm

# go-bindata'yı kur
RUN go install github.com/go-bindata/go-bindata/...@latest

WORKDIR /go/src/github.com/kubernetes-up-and-running/kuard

# Kaynakları kopyala
COPY . .

# Ortam değişkenlerini tanımla
ENV VERBOSE=0
ENV PKG=github.com/kubernetes-up-and-running/kuard
ENV ARCH=amd64
ENV VERSION=test

# Derleme komutlarını çalıştır
RUN chmod +x build/build.sh
RUN build/build.sh

# STAGE 2: Runtime
FROM alpine:latest

USER nobody:nobody
COPY --from=build /go/bin/kuard /kuard

CMD [ "/kuard" ]
