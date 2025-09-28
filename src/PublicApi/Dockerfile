#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /app
COPY . .
#COPY ["src/PublicApi/PublicApi.csproj", "./PublicApi/"]
#RUN dotnet restore "./PublicApi/PublicApi.csproj"
#COPY . .
WORKDIR "/app/src/PublicApi"
# Debugging: check where we are and what files exist
RUN echo "Current directory is:" && pwd
RUN echo "Files in current directory:" && ls
RUN echo "Recursive file list:" && ls -R
RUN dotnet restore "/app/src/PublicApi/PublicApi.csproj"

RUN dotnet build "./PublicApi.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "./PublicApi.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "PublicApi.dll"]
