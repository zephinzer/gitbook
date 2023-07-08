# Reference/Template Dockerfiles

## Golang

1. Change `%_TODO_COMMAND_%` to your command name

{% code lineNumbers="true" %}
```docker
FROM golang:1.20-alpine AS builder
RUN apk update
RUN apk add ca-certificates make
WORKDIR /app
COPY ./go.mod ./go.mod
COPY ./go.sum ./go.sum
RUN go mod download
COPY ./internal ./internal
ARG COMMAND=%_TODO_COMMAND_%
COPY ./cmd/${COMMAND} ./cmd/${COMMAND}
RUN go mod vendor
ENV CGO_ENABLED=0
RUN go build \
  -ldflags "-extldflags 'static'" \
  -mod=vendor \
  -o ./bin/${COMMAND} \
  ./cmd/${COMMAND};

FROM scratch AS service
ARG COMMAND=%_TODO_COMMAND_%
COPY --from=builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/
COPY --from=builder /app/bin/${COMMAND} /entrypoint
ENTRYPOINT [ "/entrypoint" ]
```
{% endcode %}

## Node.js

## Python
