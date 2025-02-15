﻿FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["SearchService.WebAPI/SearchService.WebAPI.csproj", "SearchService.WebAPI/"]
COPY ["CommonInitializer/CommonInitializer.csproj", "CommonInitializer/"]
COPY ["Noodles.ASPNETCore/Noodles.ASPNETCore.csproj", "Noodles.ASPNETCore/"]
COPY ["Noodles.Common/Noodles.Common.csproj", "Noodles.Common/"]
COPY ["Noodles.EventBus/Noodles.EventBus.csproj", "Noodles.EventBus/"]
COPY ["Noodles.Infrastructure/Noodles.Infrastructure.csproj", "Noodles.Infrastructure/"]
COPY ["Noodles.DomainCommons/Noodles.DomainCommons.csproj", "Noodles.DomainCommons/"]
COPY ["Noodles.Jwt/Noodles.Jwt.csproj", "Noodles.Jwt/"]
COPY ["SearchService.Infrastructure/SearchService.Infrastructure.csproj", "SearchService.Infrastructure/"]
COPY ["SearchService.Domain/SearchService.Domain.csproj", "SearchService.Domain/"]
RUN dotnet restore "SearchService.WebAPI/SearchService.WebAPI.csproj"
COPY . .
WORKDIR "/src/SearchService.WebAPI"
RUN dotnet build "SearchService.WebAPI.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "SearchService.WebAPI.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "SearchService.WebAPI.dll"]
