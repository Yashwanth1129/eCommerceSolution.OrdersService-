# Base runtime image
FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS base
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

# Build stage
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
ARG configuration=Release
WORKDIR /src

# Copy csproj files (for restore caching)
COPY eCommerceSolution.OrdersService/eCommerceSolution.OrdersService.csproj eCommerceSolution.OrdersService/
COPY BusinessLogicLayer/BusinessLogicLayer.csproj BusinessLogicLayer/
COPY DataAccessLayer/DataAccessLayer.csproj DataAccessLayer/

# Restore dependencies
RUN dotnet restore eCommerceSolution.OrdersService/eCommerceSolution.OrdersService.csproj

# Copy full source
COPY . .

# Build
RUN dotnet build eCommerceSolution.OrdersService/eCommerceSolution.OrdersService.csproj \
    -c $configuration \
    -o /app/build

# Publish
FROM build AS publish
ARG configuration=Release
RUN dotnet publish eCommerceSolution.OrdersService/eCommerceSolution.OrdersService.csproj \
    -c $configuration \
    -o /app/publish \
    /p:UseAppHost=false

# Final runtime image
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .

# Environment variables
ENV MONGODB_HOST=yashpc
ENV MONGODB_PORT=27017
ENV MONGODB_DATABASE=OrdersDatabase
ENV UsersMicroserviceName=localhost
ENV UsersMicroservicePort=8080
ENV ProductsMicroserviceName=localhost
ENV ProductsMicroservicePort=8080

ENTRYPOINT ["dotnet", "eCommerceSolution.OrdersService.dll"]