#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["src/APIs/NET6.Services.API/NET6.Services.API.csproj", "src/APIs/NET6.Services.API/"]
RUN dotnet restore "src/APIs/NET6.Services.API/NET6.Services.API.csproj"
COPY . .
WORKDIR "/src/src/APIs/NET6.Services.API"
RUN dotnet build "NET6.Services.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "NET6.Services.API.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "NET6.Services.API.dll"]