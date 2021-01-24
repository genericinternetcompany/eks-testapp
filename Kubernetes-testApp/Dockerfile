# https://hub.docker.com/_/microsoft-dotnet
FROM mcr.microsoft.com/dotnet/sdk:5.0-buster-slim AS build
WORKDIR /source

RUN apt update
RUN apt dist-upgrade -y
RUN apt install -y npm

# copy csproj and restore as distinct layers
COPY . .
RUN dotnet restore
RUN dotnet publish -c release -o /app --no-restore --no-cache

# final stage/image
FROM mcr.microsoft.com/dotnet/aspnet:5.0-buster-slim
WORKDIR /app
COPY --from=build /app .
EXPOSE 80
ENTRYPOINT ["dotnet", "Kubernetes-testApp.dll"]

# final stage/image
FROM mcr.microsoft.com/dotnet/aspnet:5.0-buster-slim-arm64v8
WORKDIR /app
COPY --from=build /app .
EXPOSE 80
ENTRYPOINT ["dotnet", "Kubernetes-testApp.dll"]
