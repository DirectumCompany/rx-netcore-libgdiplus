FROM directum/netcore:3.1-libgdiplus-pango
RUN curl -SL --output dotnet-install.sh https://dot.net/v1/dotnet-install.sh \
    && chmod +x ./dotnet-install.sh \
    && ./dotnet-install.sh --no-path --install-dir /usr/share/dotnet --channel 5.0 \
    && ./dotnet-install.sh --no-path --install-dir /usr/share/dotnet --runtime aspnetcore --channel 2.2 \
    && rm -rf dotnet-install.sh

#Set timezone
ENV TZ=Europe/Samara
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

ENV DOTNET_CLI_TELEMETRY_OPTOUT=1
