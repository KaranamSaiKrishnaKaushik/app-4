# Build stage
FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# copy csproj and restore
COPY ["dotnet-app-4/dotnet-app-4.csproj", "dotnet-app-4/"]
RUN dotnet restore "dotnet-app-4/dotnet-app-4.csproj"

# copy everything else and build
COPY . .
WORKDIR "/src/dotnet-app-4"
RUN dotnet build "dotnet-app-4.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Publish stage
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "dotnet-app-4.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Runtime stage (IMPORTANT: use aspnet, not runtime)
FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS final
WORKDIR /app
COPY --from=publish /app/publish .

# If you really need to run as non-root, uncomment these two lines:
# RUN adduser --disabled-password --gecos "" appuser && chown -R appuser /app
# USER appuser

ENTRYPOINT ["dotnet", "dotnet-app-4.dll"]
