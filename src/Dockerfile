FROM debian:bullseye-slim as binary

# IMPORTANT!
# Upstream version in github (https://github.com/bitcoin/bitcoin/releases) => v22.0
# Upstream version in offical bitcoin releases website (https://bitcoincore.org/bin/bitcoin-core-22.0/) => 22.0
# To fix this incongruation it is removed the first char from the env UPSTREA_VERSION to download the binary and
# keep the CI in github to bumpo the version with the sdk bot


RUN apt-get update && apt-get install curl -y

ARG UPSTREAM_VERSION
ARG TARGETARCH
# map Docker arch to Core tarball arch names + verify checksum and extract
RUN set -eux; \
  VERSION_NO_V="${UPSTREAM_VERSION#v}"; \
  case "$TARGETARCH" in \
  amd64)  CORE_ARCH="x86_64" ;; \
  arm64)  CORE_ARCH="aarch64" ;; \
  *) echo "Unsupported arch: $TARGETARCH" >&2; exit 1 ;; \
  esac; \
  BASE_URL="https://bitcoincore.org/bin/bitcoin-core-${VERSION_NO_V}"; \
  TARBALL="bitcoin-${VERSION_NO_V}-${CORE_ARCH}-linux-gnu.tar.gz"; \
  curl -fsSL "${BASE_URL}/${TARBALL}" -o /tmp/bitcoin.tar.gz; \
  curl -fsSL "${BASE_URL}/SHA256SUMS" -o /tmp/SHA256SUMS; \
  EXPECTED="$(grep " ${TARBALL}\$" /tmp/SHA256SUMS | awk '{print $1}')"; \
  ACTUAL="$(sha256sum /tmp/bitcoin.tar.gz | awk '{print $1}')"; \
  test -n "$EXPECTED"; \
  [ "$EXPECTED" = "$ACTUAL" ] || { echo "SHA256 mismatch: expected=$EXPECTED actual=$ACTUAL" >&2; exit 1; }; \
  mkdir -p /bitcoin && tar -xzf /tmp/bitcoin.tar.gz -C /bitcoin --strip-components=1

FROM debian:bullseye-slim AS final

EXPOSE 8332 8333 18332 18333 28332 28333

COPY --from=binary /bitcoin/bin/bitcoind /usr/local/bin/

ADD ./docker_entrypoint.sh /usr/local/bin/docker_entrypoint.sh
RUN chmod a+x /usr/local/bin/docker_entrypoint.sh

ENTRYPOINT ["/usr/local/bin/docker_entrypoint.sh"]
