FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
WORKDIR /app
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["TradingApp.sln", "."]
COPY ["src/TradingApp.Api/TradingApp.Api.csproj", "src/TradingApp.Api/"]
COPY ["src/TradingApp.Application/TradingApp.Application.csproj", "src/TradingApp.Application/"]
COPY ["src/TradingApp.Domain/TradingApp.Domain.csproj", "src/TradingApp.Domain/"]
COPY ["src/TradingApp.Infrastructure/TradingApp.Infrastructure.csproj", "src/TradingApp.Infrastructure/"]
RUN dotnet restore "src/TradingApp.Api/TradingApp.Api.csproj"
COPY . .
WORKDIR "/src/src/TradingApp.Api"
RUN dotnet build "TradingApp.Api.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
RUN dotnet publish "TradingApp.Api.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "TradingApp.Api.dll"]
