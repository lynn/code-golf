FROM alpine:3.20 AS builder

ENV VER=1.4.1

RUN apk add --no-cache curl icu-data npm

RUN curl -L https://github.com/gleam-lang/gleam/releases/download/v$VER/gleam-v$VER-x86_64-unknown-linux-musl.tar.gz | tar xz

FROM codegolf/lang-base

COPY --from=0 /bin/cat /bin/chmod           \
              /bin/cp  /bin/mkdir /bin/sh   /bin/
COPY --from=0 /lib/ld-musl-x86_64.so.1      \
              /lib/libcrypto.so.3           \
              /lib/libssl.so.3              \
              /lib/libz.so.1                /lib/
COPY --from=0 /gleam /usr/bin/node          /usr/bin/
COPY --from=0 /usr/lib/libada.so.2          \
              /usr/lib/libbrotlicommon.so.1 \
              /usr/lib/libbrotlidec.so.1    \
              /usr/lib/libbrotlienc.so.1    \
              /usr/lib/libcares.so.2        \
              /usr/lib/libgcc_s.so.1        \
              /usr/lib/libicudata.so.74     \
              /usr/lib/libicui18n.so.74     \
              /usr/lib/libicuuc.so.74       \
              /usr/lib/libnghttp2.so.14     \
              /usr/lib/libstdc++.so.6       \
              /usr/lib/libuv.so.1           /usr/lib/
COPY --from=0 /usr/share/icu/74.2/icu*.dat  /usr/share/icu/74.2/

COPY /gleamwrapper /usr/bin/
# Now you can execute again. Remove the next line when wrapping up.
RUN chmod +x /usr/bin/gleamwrapper

RUN chmod 777 /tmp

USER nobody

COPY --chown=nobody /gleam.toml /gleam/

WORKDIR /gleam

RUN HOME=/tmp gleam deps download

ENTRYPOINT ["gleamwrapper"]

CMD ["--version"]
