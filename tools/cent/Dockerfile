FROM golang:alpine AS builder
LABEL org.opencontainers.image.source="https://github.com/cosasdepuma/migaloo"
COPY ./origin/ /origin
WORKDIR /origin
RUN /usr/local/go/bin/go build -o /tool .

FROM alpine AS system
RUN /sbin/apk update && /sbin/apk add --no-cache ca-certificates && /usr/sbin/update-ca-certificates

FROM scratch
COPY --from=system /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/
COPY --from=builder /tool /app/tool
COPY --from=builder /origin/.cent.yaml /app/.cent.yaml
ENV HOME /app
VOLUME ["/out"]
ENTRYPOINT ["/app/tool", "--config", "/app/.cent.yaml", "--path", "/out"]
