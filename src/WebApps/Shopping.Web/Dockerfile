FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

USER app
FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG configuration=Release
WORKDIR /src
COPY ["WebApps/Shopping.Web/Shopping.Web.csproj", "WebApps/Shopping.Web/"]
RUN dotnet restore "WebApps/Shopping.Web/Shopping.Web.csproj"
COPY . .
WORKDIR "/src/WebApps/Shopping.Web"
RUN dotnet build "Shopping.Web.csproj" -c $configuration -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "Shopping.Web.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Shopping.Web.dll"]
