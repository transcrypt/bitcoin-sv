FROM alpine

LABEL maintainer="Anton Kulikov (anton.kulikov@tsrpay.com)"

RUN set -ex \
    && apk --no-cache add su-exec

ARG BITCOIN_SV_VERSION=0.1.1
ARG BITCOIN_SV_DIR=bitcoin-sv-${BITCOIN_SV_VERSION}
ARG BITCOIN_SV_DIST=bitcoin-sv-${BITCOIN_SV_VERSION}-x86_64-linux-gnu.tar.gz
ARG BITCOIN_SV_SHA256=6a537c7b050594c3f4e674abfd925dfdf6d267af530a83581354362589dcfb79
ARG BITCOIN_SV_SHASUM="${BITCOIN_SV_SHA256}  ${BITCOIN_SV_DIST}"
ARG BITCOIN_SV_URL=https://download.bitcoinsv.io/bitcoinsv/${BITCOIN_SV_VERSION}/${BITCOIN_SV_DIST}
ARG BITCOIN_SV_DATA=/home/bitcoin/.bitcoin

RUN wget -O ${BITCOIN_SV_DIST} ${BITCOIN_SV_URL} \
    && echo "${BITCOIN_SV_SHASUM}" | sha256sum -c \
    && tar -xzf ${BITCOIN_SV_DIST} \
    && mv ${BITCOIN_SV_DIR}/bin/bitcoind ${BITCOIN_SV_DIR}/bin/bitcoin-cli ${BITCOIN_SV_DIR}/bin/bitcoin-tx /usr/local/bin/ \
    && rm -rf ${BITCOIN_SV_DIR} \
    && rm ${BITCOIN_SV_DIST} \
    && addgroup -S bitcoin && adduser -S -G bitcoin bitcoin

# Install glibc binaries needed by bitcoin-sv
RUN apk --no-cache --allow-untrusted -X https://apkproxy.herokuapp.com/sgerrand/alpine-pkg-glibc add glibc glibc-bin

COPY docker-entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

VOLUME ["/home/bitcoin/.bitcoin"]

EXPOSE 8332 8333 18332 18333 18444

ENTRYPOINT ["/entrypoint.sh"]

CMD ["bitcoind"]
