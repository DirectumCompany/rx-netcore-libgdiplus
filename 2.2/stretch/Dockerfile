FROM microsoft/dotnet:2.2-aspnetcore-runtime
RUN apt-get update \
    && apt-get install -y --no-install-recommends libgdiplus libc6-dev locales \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*
# Установка локализации в контейнере
RUN echo "ru_RU.CP866 IBM866" >> /etc/locale.gen \
    && echo "ru_RU.UTF-8 UTF-8" >> /etc/locale.gen \
    && locale-gen 