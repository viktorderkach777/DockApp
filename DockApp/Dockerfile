FROM microsoft/dotnet:2.2-aspnetcore-runtime AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

RUN curl -sL https://deb.nodesource.com/setup_10.x |  bash -
RUN apt-get install -y nodejs

FROM microsoft/dotnet:2.2-sdk AS build
WORKDIR /src
COPY ["DockApp/DockApp.csproj", "DockApp/"]
RUN dotnet restore "DockApp/DockApp.csproj"
COPY . .
WORKDIR "/src/DockApp"
RUN dotnet build "DockApp.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "DockApp.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "DockApp.dll"]