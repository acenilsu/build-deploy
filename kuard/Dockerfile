# STAGE 1: Build
FROM golang:1.20-alpine AS build

# Gerekli bağımlılıkları kur
RUN apk update && apk add --no-cache git bash nodejs npm

# Ortam değişkenlerini ayarla
ENV NODE_OPTIONS=--openssl-legacy-provider
ENV GO111MODULE=on

# go-bindata ve bağımlılıkları kur
RUN go install github.com/go-bindata/go-bindata/...@latest

WORKDIR /go/src/github.com/kubernetes-up-and-running/kuard

# Kaynak dosyaları kopyala
COPY . .

# Node.js bağımlılıklarını yükle
WORKDIR /go/src/github.com/kubernetes-up-and-running/kuard/client
RUN npm install
RUN npm run build

# Go uygulamasını derle
WORKDIR /go/src/github.com/kubernetes-up-and-running/kuard
RUN chmod +x build/build.sh
RUN ./build/build.sh

# STAGE 2: Runtime
FROM alpine:latest

USER nobody:nobody
COPY --from=build /go/bin/kuard /kuard

CMD [ "/kuard" ]
