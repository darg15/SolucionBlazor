#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

ENV DE_CONNECTION_STRING=["Server=192.168.100.7,1433;Database=DbCrudBlazor;User ID = sa; Password=6151;TrustServerCertificate=True;"]

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["BlazorCrud.Server/BlazorCrud.Server.csproj", "BlazorCrud.Server/"]
RUN dotnet restore "./BlazorCrud.Server/BlazorCrud.Server.csproj"
COPY . .
WORKDIR "/src/BlazorCrud.Server"
RUN dotnet build "./BlazorCrud.Server.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./BlazorCrud.Server.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "BlazorCrud.Server.dll"]