FROM directum/netcore:3.1-libgdiplus-pango

# Ставим криптопро и прибираем за собой.
COPY . /tmp/src
RUN	cd /tmp/src \
    && md5sum -c md5sums.txt \
    && tar -xf cryptopro-linux-amd64_deb.tgz \
    && linux-amd64_deb/install.sh \
    && rm -rf /tmp/src

# Делаем симлинки на утилиты криптопро.
RUN cd /bin \
    && ln -s /opt/cprocsp/bin/amd64/certmgr \
    && ln -s /opt/cprocsp/bin/amd64/cpverify \
    && ln -s /opt/cprocsp/bin/amd64/cryptcp \
    && ln -s /opt/cprocsp/bin/amd64/csptest \
    && ln -s /opt/cprocsp/bin/amd64/csptestf \
    && ln -s /opt/cprocsp/bin/amd64/der2xer \
    && ln -s /opt/cprocsp/bin/amd64/inittst \
    && ln -s /opt/cprocsp/bin/amd64/wipefile \
    && ln -s /opt/cprocsp/sbin/amd64/cpconfig

# Ставим расширение ГОСТ для openssl и прибираем за собой.
RUN apt-get update \
    && apt-get install -y libengine-gost-openssl1.1 \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Конфигурируем openssl на работу с расширением ГОСТ.
RUN sed -i "/^\[default_conf\]/a engines = engine_section" /usr/lib/ssl/openssl.cnf \
    && echo "\n[engine_section]\ngost = gost_section\n\n[gost_section]\nengine_id = gost\ndefault_algorithms = ALL\nCRYPT_PARAMS = id-Gost28147-89-CryptoPro-A-ParamSet" >> /usr/lib/ssl/openssl.cnf
