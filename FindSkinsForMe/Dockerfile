FROM mcr.microsoft.com/dotnet/runtime:8.0 AS base
USER $APP_UID
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["FindSkinsForMe/FindSkinsForMe.csproj", "FindSkinsForMe/"]
RUN dotnet restore "FindSkinsForMe/FindSkinsForMe.csproj"
COPY . .
WORKDIR "/src/FindSkinsForMe"
RUN dotnet build "FindSkinsForMe.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "FindSkinsForMe.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "FindSkinsForMe.dll"]
