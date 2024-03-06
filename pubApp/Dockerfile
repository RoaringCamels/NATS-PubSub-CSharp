FROM mcr.microsoft.com/dotnet/framework/sdk:4.8 AS build
WORKDIR /src
COPY . .

# Run MSBuild directly
RUN msbuild /p:Configuration=Release

FROM mcr.microsoft.com/dotnet/framework/aspnet:4.8 AS runtime
WORKDIR /inetpub/wwwroot
COPY --from=build /src/bin/Release/Publish .

ENTRYPOINT ["dotnet", "pubApp.dll"]

