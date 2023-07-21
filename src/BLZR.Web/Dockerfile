#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:8.0-preview AS base
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0-preview AS build
WORKDIR /src
COPY ["src/BLZR.Web/BLZR.Web.csproj", "src/BLZR.Web/"]
RUN dotnet restore "src/BLZR.Web/BLZR.Web.csproj"
COPY . .
WORKDIR "/src/src/BLZR.Web"
RUN dotnet build "BLZR.Web.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "BLZR.Web.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "BLZR.Web.dll"]