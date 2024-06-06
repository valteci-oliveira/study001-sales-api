FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 5247

ENV ASPNETCORE_URLS=http://+:5247

USER app
FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG configuration=Release
WORKDIR /src
COPY ["study001-sales-api.csproj", "./"]
RUN dotnet restore "study001-sales-api.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "study001-sales-api.csproj" -c $configuration -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "study001-sales-api.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "study001-sales-api.dll"]
