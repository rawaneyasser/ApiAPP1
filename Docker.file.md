# https://hub.docker.com/_/microsoft-dotnet-core

FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS build

WORKDIR /CalcApiApp1

# copy csproj and restore as distinct layers

COPY *.sln .

COPY CalcApiApp1/*.csproj ./CalcApiApp1/

RUN dotnet restore

# copy everything else and build app

COPY CalcApiApp1/. ./CalcApiApp1/

WORKDIR /CalcApiApp1

RUN dotnet publish -c release -o /app --no-restore

# final stage/image

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1

WORKDIR /app

COPY --from=build /app ./

ENTRYPOINT ["dotnet", "CalcApiAp1p.dll"]