FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
RUN apt-get update && apt-get install -y \
    curl

WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0.200 AS build
WORKDIR /src
COPY "MvcApp.csproj" .
RUN dotnet restore "MvcApp.csproj"
COPY . .
RUN dotnet build . -c Release -o /app/build

# RUN apt-get install curl

FROM build AS publish
RUN dotnet publish "MvcApp.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "MvcApp.dll"]