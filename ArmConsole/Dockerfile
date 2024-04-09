# FROM mcr.microsoft.com/dotnet/runtime:7.0 AS base
FROM mcr.microsoft.com/dotnet/runtime:7.0-cbl-mariner2.0-arm64v8 AS base
WORKDIR /app

# FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
FROM mcr.microsoft.com/dotnet/sdk:7.0-cbl-mariner2.0-arm64v8 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["ArmConsole/ArmConsole.csproj", "ArmConsole/"]
RUN dotnet restore "ArmConsole/ArmConsole.csproj"
COPY . .
WORKDIR "/src/ArmConsole"
RUN dotnet build "ArmConsole.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "ArmConsole.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ArmConsole.dll"]
