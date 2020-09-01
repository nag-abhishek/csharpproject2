FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim
WORKDIR /app
COPY  app/bin/Release/netcoreapp3.1/publish .
ENTRYPOINT ["dotnet", "WebApplication4.dll"]
EXPOSE 80
EXPOSE 443