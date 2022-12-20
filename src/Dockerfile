FROM debian:bullseye-slim as binary

# IMPORTANT!
# Upstream version in github (https://github.com/bitcoin/bitcoin/releases) => v22.0
# Upstream version in offical bitcoin releases website (https://bitcoincore.org/bin/bitcoin-core-22.0/) => 22.0
# To fix this incongruation it is removed the first char from the env UPSTREA_VERSION to download the binary and
# keep the CI in github to bumpo the version with the sdk bot

ARG UPSTREAM_VERSION

RUN apt-get update && apt-get install curl -y

RUN curl https://bitcoincore.org/bin/bitcoin-core-${UPSTREAM_VERSION}/bitcoin-${UPSTREAM_VERSION}-x86_64-linux-gnu.tar.gz -o bitcoin-${UPSTREAM_VERSION}-x86_64-linux-gnu.tar.gz

RUN expected_256checksum=$(curl https://bitcoincore.org/bin/bitcoin-core-${UPSTREAM_VERSION}/SHA256SUMS | grep bitcoin-${UPSTREAM_VERSION}-x86_64-linux-gnu.tar.gz | awk '{print $1}')  \
  && if [ sha256sum /bitcoin-${UPSTREAM_VERSION}-x86_64-linux-gnu.tar.gz | awk '{print $1}'  != "$expected_256checksum" ]; then \
  tar xzvf /bitcoin-${UPSTREAM_VERSION}-x86_64-linux-gnu.tar.gz; \
  mv /bitcoin-${UPSTREAM_VERSION} /bitcoin; \
  echo "Checksum verification failed."; \
  else \
  tar xzvf /bitcoin-${UPSTREAM_VERSION}-x86_64-linux-gnu.tar.gz; \
  mv /bitcoin-${UPSTREAM_VERSION} /bitcoin; \
  fi



FROM debian:bullseye-slim AS final

EXPOSE 8332 8333 18332 18333 28332 28333

COPY --from=binary /bitcoin/bin/bitcoind /usr/local/bin/

ADD ./docker_entrypoint.sh /usr/local/bin/docker_entrypoint.sh
RUN chmod a+x /usr/local/bin/docker_entrypoint.sh

ENTRYPOINT ["/usr/local/bin/docker_entrypoint.sh"]