#base stage
FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
EXPOSE 80
EXPOSE 443
WORKDIR /app


#build stage
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["Library/Library.csproj", "Library/"]
COPY ["Library.Data/Library.Data.csproj", "Library.Data/"]
RUN dotnet restore "Library/Library.csproj"
COPY . .
WORKDIR /src/Library
RUN dotnet build "Library.csproj" -c Release -o /app/build

#publish stage
FROM build AS publish
RUN dotnet publish "Library.csproj" -c Release -o /app/publish /p:UseAppHostfile=false


#finish stage
FROM publish AS final
WORKDIR /app
COPY --from=publish /app/publish/ .
ENTRYPOINT [ "dotnet", "Library.dll" ]