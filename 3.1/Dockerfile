FROM mcr.microsoft.com/dotnet/core/aspnet:3.1
RUN apt-get update \
    && apt-get install -y --no-install-recommends krb5-user \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*
