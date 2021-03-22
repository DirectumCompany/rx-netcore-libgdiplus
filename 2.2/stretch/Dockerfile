FROM microsoft/dotnet:2.2-aspnetcore-runtime
RUN apt-get update \
    && apt-get install -y --no-install-recommends libgdiplus libc6-dev language-pack-ru\
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*
# Установка локализации в контейнере
ENV LANGUAGE ru_RU.UTF-8
ENV LANG ru_RU.UTF-8
ENV LC_ALL ru_RU.UTF-8
RUN echo "ru_RU.CP866 IBM866" >> /etc/locale.gen \
    && locale-gen