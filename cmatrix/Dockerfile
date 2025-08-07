# Build Container Image
FROM alpine as cmatrixbuilder

LABEL org.opencontainers.image.authors="Carlos Engels"

WORKDIR cmatrix

RUN apk update --no-cache && \
    apk add git autoconf automake alpine-sdk ncurses-dev ncurses-static && \
    git clone https://github.com/spurin/cmatrix.git . && \
    autoreconf -i && \ 
    mkdir -p /usr/lib/kbd/consolefonts /usr/share/consolefonts && \ 
    ./configure LDFLAGS="-static" && \ 
    make

# cmatrix Container Imager
FROM alpine 

LABEL org.opencontainers.image.authors="Carlos Engels"

RUN apk update --no-cache && \
    apk add ncurses-terminfo-base && \
    adduser -g "Thomas Anderson" -s /usr/sbin/nologin -D -H thomas
COPY --from=cmatrixbuilder /cmatrix/cmatrix /cmatrix

USER thomas
ENTRYPOINT ["./cmatrix"]
CMD ["-b"]
