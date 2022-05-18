FROM debian:bullseye-slim as binary

ARG UPSTREAM_VERSION=0.20.0

ENV FILENAME bitcoin-${UPSTREAM_VERSION}-x86_64-linux-gnu.tar.gz
ENV DOWNLOAD_URL https://bitcoin.org/bin/bitcoin-core-${UPSTREAM_VERSION}/${FILENAME}
ENV SHA256SUM 35ec10f87b6bc1e44fd9cd1157e5dfa483eaf14d7d9a9c274774539e7824c427

ADD $DOWNLOAD_URL /$FILENAME
RUN if [ x"$( sha256sum /${FILENAME} | awk '{print $1}' )" != x"${SHA256SUM}" ]; then \
  rm -f /$FILENAME; \
  echo "Checksum verification failed."; \
  exit 1; \
  else \
  tar xzvf /$FILENAME; \
  mv /bitcoin-${UPSTREAM_VERSION} /bitcoin; \
  fi

FROM debian:bullseye-slim AS final

EXPOSE 8332 8333 18332 18333 28332 28333

COPY --from=binary /bitcoin/bin/bitcoind /usr/local/bin/

ADD ./bin/docker_entrypoint.sh /usr/local/bin/docker_entrypoint.sh
RUN chmod a+x /usr/local/bin/docker_entrypoint.sh

ENTRYPOINT ["/usr/local/bin/docker_entrypoint.sh"]
