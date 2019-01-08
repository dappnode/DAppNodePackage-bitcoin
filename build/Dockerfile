FROM alpine as binary

ARG VERSION=0.17.1

ENV FILENAME bitcoin-${VERSION}-x86_64-linux-gnu.tar.gz
ENV DOWNLOAD_URL https://bitcoin.org/bin/bitcoin-core-${VERSION}/${FILENAME}
ENV SHA256SUM 6ccc675ee91522eee5785457e922d8a155e4eb7d5524bd130eb0ef0f0c4a6008

ADD $DOWNLOAD_URL /$FILENAME
RUN if [ x"$( sha256sum /${FILENAME} | awk '{print $1}' )" != x"${SHA256SUM}" ]; then \
      rm -f /$FILENAME; \
      echo "Checksum verification failed."; \
      exit 1; \
    else \
      tar xzvf /$FILENAME; \
      mv /bitcoin-${VERSION::-2} /bitcoin; \
    fi

FROM alpine AS final

ARG GLIBC_VERSION=2.28-r0

RUN apk update \
  && apk --no-cache add wget bash ca-certificates \
  && wget -q -O /etc/apk/keys/sgerrand.rsa.pub https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub \
  && wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/${GLIBC_VERSION}/glibc-${GLIBC_VERSION}.apk \
  && wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/${GLIBC_VERSION}/glibc-bin-${GLIBC_VERSION}.apk \
  && apk --no-cache add glibc-${GLIBC_VERSION}.apk \
  && apk --no-cache add glibc-bin-${GLIBC_VERSION}.apk \
  && rm -rf /glibc-${GLIBC_VERSION}.apk \
  && rm -rf /glibc-bin-${GLIBC_VERSION}.apk \
  && mkdir /root/.bitcoin \
  && apk del wget ca-certificates

EXPOSE 8332 8333 18332 18333 28332 28333

COPY --from=binary /bitcoin/bin/bitcoind /usr/local/bin/

ADD ./bin/docker_entrypoint.sh /usr/local/bin/docker_entrypoint.sh
RUN chmod a+x /usr/local/bin/docker_entrypoint.sh

ENTRYPOINT ["/usr/local/bin/docker_entrypoint.sh"]
