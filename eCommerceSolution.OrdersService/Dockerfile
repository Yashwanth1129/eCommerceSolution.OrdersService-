FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS base
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
ARG configuration=Release
WORKDIR /src

COPY ["eCommerceSolution.OrdersService.csproj", "./"]
RUN dotnet restore "eCommerceSolution.OrdersService.csproj"

COPY . .
RUN dotnet build "eCommerceSolution.OrdersService.csproj" -c $configuration -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "eCommerceSolution.OrdersService.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .

ENV MONGODB_HOST=yashpc
ENV MONGODB_PORT=27017
ENV MONGODB_DATABASE=OrdersDatabase
ENV UsersMicroserviceName=localhost
ENV UsersMicroservicePort=8080
ENV ProductsMicroserviceName=localhost
ENV ProductsMicroservicePort=8080

ENTRYPOINT ["dotnet", "eCommerceSolution.OrdersService.dll"]





