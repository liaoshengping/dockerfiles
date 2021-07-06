FROM golang

#设置时区
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

ENV GO111MODULE on

WORKDIR /go/src/app

RUN apk update \
  && apk add git \
  && go install github.com/cosmtrek/air@latest \
  && go get -u golang.org/x/tools/gopls \
  github.com/ramya-rao-a/go-outline