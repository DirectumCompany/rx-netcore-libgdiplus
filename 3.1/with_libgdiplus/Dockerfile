# TODO: В репозиториях testing лежит новая версия libgdiplus 6.04, на которой не проявляется ошибка с нехваткой памяти.
# В будущем можно перейти на эту версию и не патчить исходники (сейчас если подключить testing, размер образа увеличивается на ~100Мб).

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1 as builder
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
     libpng-dev libgif-dev git autoconf libtool automake build-essential gettext libglib2.0-dev libcairo2-dev libtiff-dev libexif-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /usr/local/share
RUN git clone https://github.com/mono/libgdiplus

# HACK: patch max memory size
# см. [https://forum.aspose.com/t/pdf-to-image-dotnetcore-2-1-error-region-cgdipcombineregionpath-assertion-failed-region-bitmap/178516/28]
WORKDIR /usr/local/share/libgdiplus/src
RUN sed -i 's/1024/10240/g' region-bitmap.h

WORKDIR /usr/local/share/libgdiplus
RUN ./autogen.sh \
    && make \
    && make install

WORKDIR /usr/local/share
RUN rm -r libgdiplus

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1
RUN echo ttf-mscorefonts-installer msttcorefonts/accepted-mscorefonts-eula \
    select true | debconf-set-selections
# Репозиторий contrib нужен для ttf-mscorefonts-installer
RUN echo 'deb http://deb.debian.org/debian buster contrib' >> /etc/apt/sources.list \
    && apt-get update \
    && apt-get install -y --no-install-recommends ttf-mscorefonts-installer gss-ntlmssp libc6-dev \
     libgif-dev libglib2.0-dev libcairo2-dev libtiff-dev libexif-dev locales gnupg1 \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*
COPY --from=builder /usr/local/lib/libgdiplus* /usr/lib/

# Установка локализации в контейнере
ENV LANGUAGE ru_RU.UTF-8
ENV LANG ru_RU.UTF-8
ENV LC_ALL ru_RU.UTF-8
RUN echo "ru_RU.CP866 IBM866" >> /etc/locale.gen \
    && locale-gen

# Очищаем предопределенные адреса прослушивания ASP.NET Core, чтобы не было warning-ов при старте сервисов.
ENV ASPNETCORE_URLS=
