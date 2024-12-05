# STAGE 1: Build
FROM golang:1.20-alpine AS build

# Gerekli bağımlılıkları kur
RUN apk update && apk add --no-cache git bash npm dos2unix

# Go modüllerini aktif et
ENV GO111MODULE=on

# go-bindata'yı kur
RUN go install github.com/go-bindata/go-bindata/...@latest

WORKDIR /go/src/github.com/kubernetes-up-and-running/kuard

# Kaynakları kopyala
COPY . .

# Betik dosyasını Unix formatına çevir ve izinleri ayarla
RUN dos2unix build/build.sh
RUN chmod +x build/build.sh

# Ortam değişkenlerini tanımla
ENV VERBOSE=0
ENV PKG=github.com/kubernetes-up-and-running/kuard
ENV ARCH=amd64
ENV VERSION=test

# Derleme işlemini gerçekleştir
RUN ./build/build.sh

# STAGE 2: Runtime
FROM alpine:latest

USER nobody:nobody

# Çıktı dosyasını kopyala
COPY --from=build /go/bin/kuard /kuard

# Uygulamayı başlat
CMD [ "/kuard" ]
