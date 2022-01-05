#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["SqlNet6Api/SqlNet6Api.csproj", "SqlNet6Api/"]
RUN dotnet restore "SqlNet6Api/SqlNet6Api.csproj"
COPY . .
WORKDIR "/src/SqlNet6Api"
RUN dotnet build "SqlNet6Api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "SqlNet6Api.csproj" -c Release -o /app/publish

FROM base AS final

ENV SERVERNAME=""
ENV DATABASE=""
ENV USERNAME=""
ENV PASSWORD=""

WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "SqlNet6Api.dll"]