FROM microsoft/dotnet:2.1-aspnetcore-runtime AS base
WORKDIR /app
ENV ASPNETCORE_URLS http://+:5000
EXPOSE 5000

FROM microsoft/dotnet:2.1-sdk AS build
ARG Configuration=Release
WORKDIR /src
COPY *.sln ./

COPY api/api.csproj api/
COPY shared/shared.csproj shared/
RUN dotnet restore 
COPY . .
WORKDIR "/src/api"
RUN dotnet build -c $Configuration -o /app

FROM build AS publish
ARG Configuration=Release
RUN dotnet publish -c $Configuration -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "api.dll"]
