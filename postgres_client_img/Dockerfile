FROM alpine:latest

RUN apk update && \
    apk add --no-cache \
    postgresql-client \
    bash \
    shadow

RUN addgroup -S pgapp && \
    adduser -S -G pgapp -h /pgapp pgapp &&\
    chown -R pgapp:pgapp pgapp

USER pgapp
SHELL [ "/bin/bash" ]

CMD [ "sleep","infinit" ]