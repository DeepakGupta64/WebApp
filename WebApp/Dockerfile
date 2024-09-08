#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

# Use the ASP.NET Core runtime image as the base for the final image
FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

# Use the SDK image to build the application
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src

# Copy the project file and restore any dependencies
COPY ["WebApp/WebApp.csproj", "./WebApp/"]
RUN dotnet restore "./WebApp/WebApp.csproj"

# Copy the remaining files and build the application
COPY . .
WORKDIR "/src/WebApp"
RUN dotnet build "WebApp.csproj" -c Release -o /app/build

# Publish the application to the /app/publish directory
FROM build AS publish
RUN dotnet publish "WebApp.csproj" -c Release -o /app/publish /p:UseAppHost=false

# Final image to run the app
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WebApp.dll"]
