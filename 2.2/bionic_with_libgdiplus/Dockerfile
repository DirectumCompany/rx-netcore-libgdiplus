FROM mcr.microsoft.com/dotnet/core/aspnet:2.2-bionic
RUN echo ttf-mscorefonts-installer msttcorefonts/accepted-mscorefonts-eula \
    select true | debconf-set-selections
RUN apt-get update \
    && apt-get install -y --no-install-recommends ttf-mscorefonts-installer gss-ntlmssp libc6-dev \
     language-pack-ru gnupg1 libgdiplus \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* 

# Установка локализации в контейнере
ENV LANGUAGE ru_RU.UTF-8
ENV LANG ru_RU.UTF-8
ENV LC_ALL ru_RU.UTF-8
RUN echo "ru_RU.CP866 IBM866" >> /etc/locale.gen \
    && locale-gen

# Очищаем предопределенные адреса прослушивания ASP.NET Core, чтобы не было warning-ов при старте сервисов.
ENV ASPNETCORE_URLS=
