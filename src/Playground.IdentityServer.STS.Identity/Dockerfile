FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["src/Playground.IdentityServer.STS.Identity/Playground.IdentityServer.STS.Identity.csproj", "src/Playground.IdentityServer.STS.Identity/"]
COPY ["src/Playground.IdentityServer.Admin.EntityFramework.Shared/Playground.IdentityServer.Admin.EntityFramework.Shared.csproj", "src/Playground.IdentityServer.Admin.EntityFramework.Shared/"]
COPY ["src/Playground.IdentityServer.Shared/Playground.IdentityServer.Shared.csproj", "src/Playground.IdentityServer.Shared/"]
RUN dotnet restore "src/Playground.IdentityServer.STS.Identity/Playground.IdentityServer.STS.Identity.csproj"
COPY . .
WORKDIR "/src/src/Playground.IdentityServer.STS.Identity"
RUN dotnet build "Playground.IdentityServer.STS.Identity.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Playground.IdentityServer.STS.Identity.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENV ASPNETCORE_FORWARDEDHEADERS_ENABLED=true
ENTRYPOINT ["dotnet", "Playground.IdentityServer.STS.Identity.dll"]