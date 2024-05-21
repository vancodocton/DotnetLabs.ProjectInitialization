#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/runtime:8.0 AS base
USER app
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["src/Worker/Demo.Worker.csproj", "src/Worker/"]
COPY ["src/Core/Demo.Core.csproj", "src/Core/"]
COPY ["src/Infrastucture/Demo.Infrastructure.csproj", "src/Infrastucture/"]
RUN dotnet restore "./src/Worker/Demo.Worker.csproj"
COPY . .
WORKDIR "/src/src/Worker"
RUN dotnet build "./Demo.Worker.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./Demo.Worker.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Demo.Worker.dll"]