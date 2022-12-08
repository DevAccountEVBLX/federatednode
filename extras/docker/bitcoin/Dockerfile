FROM counterparty/base

MAINTAINER Electra Protocol Developers <team@electraprotocol.com>

# install bitcoin core
ENV BITCOIN_VER="1.0.4.0"
ENV BITCOIN_FOLDER_VER="1.0.4.0"
ENV BITCOIN_SHASUM="C787647ACDB19045A3F98910BF9F69EF23CB4EA330412702E067EC433AA2BB9B"
WORKDIR /tmp

RUN wget --no-check-certificate -O Linux_${BITCOIN_VER}_CLI.tar.gz https://github.com/ElectraProtocol/XEP-Core/releases/download/v{BITCOIN_VER}/Linux_{BITCOIN_VER}_CLI.tar.gz

RUN myhash=$(sha256sum "Linux_${BITCOIN_VER}_CLI.tar.gz" | cut -d' ' -f1); \
    if [ "$myhash" = "$BITCOIN_SHASUM" ] ; \
        then echo "checksum ok"; \
        else echo "checksum failed for Linux_${BITCOIN_VER}_CLI.tar.gz"; exit 255 ; \
    fi
RUN tar -xvzf Linux_${BITCOIN_VER}_CLI.tar.gz
RUN install -C -m 755 -o root -g root --backup=off Linux_${BITCOIN_FOLDER_VER}_CLI/* /usr/local/bin/
RUN rm Linux_${BITCOIN_VER}_CLI.tar.gz && rm -rf Linux_${BITCOIN_FOLDER_VER}_CLI

# Set up bitcoind dirs and files
RUN mkdir -p /root/.xep/
COPY xep.conf /root/.xep/
COPY start.sh /usr/local/bin/start.sh
RUN chmod a+x /usr/local/bin/start.sh

EXPOSE 16816 16817 18332 18333

# NOTE: Defaults to running on mainnet, specify -e TESTNET=1 to start up on testnet
ENTRYPOINT ["start.sh"]
