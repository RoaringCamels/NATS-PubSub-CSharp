## Creating containers for pub/sub
1. dotnet new console -n PublisherApp
2. cd PublisherApp
3. dotnet add package NATS.Client

<br>

4. dotnet new console -n SubscriberApp
5. cd SubscriberApp
6. dotnet add package NATS.Client

<br>

7. Docker file for both Apps
```
FROM mcr.microsoft.com/dotnet/core/runtime:3.1 AS base
WORKDIR /app

FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS build
WORKDIR /src
COPY . .

RUN dotnet restore
RUN dotnet publish -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=build /app .
ENTRYPOINT ["dotnet", "PublisherApp.dll"]
```

8. docker-compose.yml in root
```
version: '3'
services:
  nats-server:
    image: 'nats:latest'
    ports:
      - '4222:4222'

  publisher:
    build:
      context: ./PublisherApp
    depends_on:
      - nats-server

  subscriber:
    build:
      context: ./SubscriberApp
    depends_on:
      - nats-server
```
