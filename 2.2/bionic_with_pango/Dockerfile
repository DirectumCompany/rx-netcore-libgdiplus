FROM mcr.microsoft.com/dotnet/core/aspnet:2.2-bionic as builder
RUN apt-get update \
    && apt-get install -y --no-install-recommends libpango1.0-dev libc6-dev \
     libgif-dev git autoconf libtool automake build-essential libglib2.0-dev libcairo2-dev libtiff-dev libexif-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*
RUN git clone https://github.com/mono/libgdiplus
WORKDIR /libgdiplus
RUN ./autogen.sh --with-pango \
    && make \
    && make install

FROM mcr.microsoft.com/dotnet/core/aspnet:2.2-bionic
RUN echo ttf-mscorefonts-installer msttcorefonts/accepted-mscorefonts-eula \
    select true | debconf-set-selections
RUN apt-get update \
    && apt-get install -y --no-install-recommends ttf-mscorefonts-installer gss-ntlmssp libpango1.0-dev libc6-dev \
     libgif-dev libglib2.0-dev libcairo2-dev libtiff-dev libexif-dev language-pack-ru \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*        
COPY --from=builder /usr/local/lib/libgdiplus* /usr/lib/
# Установка локализации в контейнере
ENV LANGUAGE ru_RU.UTF-8
ENV LANG ru_RU.UTF-8
ENV LC_ALL ru_RU.UTF-8
RUN echo "ru_RU.CP866 IBM866" >> /etc/locale.gen \
    && locale-gen

# Очищаем предопределенные адреса прослушивания ASP.NET Core, что бы не было warning-ов при старте сервисов.
ENV ASPNETCORE_URLS=
