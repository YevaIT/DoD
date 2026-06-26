# Runtime image
FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
WORKDIR /app

EXPOSE 8080

# Build image
FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Release

WORKDIR /src

COPY ["Erasmus_SSC/Erasmus_SSC/Erasmus_SSC.csproj", "Erasmus_SSC/Erasmus_SSC/"]
COPY ["Erasmus_SSC/Erasmus_SSC.Client/Erasmus_SSC.Client.csproj", "Erasmus_SSC/Erasmus_SSC.Client/"]

RUN dotnet restore "./Erasmus_SSC/Erasmus_SSC/Erasmus_SSC.csproj"

COPY . .

WORKDIR "/src/Erasmus_SSC/Erasmus_SSC"

RUN dotnet build "./Erasmus_SSC.csproj" \
    -c $BUILD_CONFIGURATION \
    -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release

RUN dotnet publish "./Erasmus_SSC.csproj" \
    -c $BUILD_CONFIGURATION \
    -o /app/publish \
    /p:UseAppHost=false

# Final production image
FROM base AS final

WORKDIR /app

COPY --from=publish /app/publish .

USER app

ENTRYPOINT ["dotnet", "Erasmus_SSC.dll"]