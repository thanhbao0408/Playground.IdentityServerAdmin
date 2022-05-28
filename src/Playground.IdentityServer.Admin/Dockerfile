FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["src/Playground.IdentityServer.Admin/Playground.IdentityServer.Admin.csproj", "src/Playground.IdentityServer.Admin/"]
COPY ["src/Playground.IdentityServer.Admin.EntityFramework.Shared/Playground.IdentityServer.Admin.EntityFramework.Shared.csproj", "src/Playground.IdentityServer.Admin.EntityFramework.Shared/"]
COPY ["src/Playground.IdentityServer.Admin.EntityFramework.SqlServer/Playground.IdentityServer.Admin.EntityFramework.SqlServer.csproj", "src/Playground.IdentityServer.Admin.EntityFramework.SqlServer/"]
COPY ["src/Playground.IdentityServer.Shared/Playground.IdentityServer.Shared.csproj", "src/Playground.IdentityServer.Shared/"]
COPY ["src/Playground.IdentityServer.Admin.EntityFramework.PostgreSQL/Playground.IdentityServer.Admin.EntityFramework.PostgreSQL.csproj", "src/Playground.IdentityServer.Admin.EntityFramework.PostgreSQL/"]
COPY ["src/Playground.IdentityServer.Admin.EntityFramework.MySql/Playground.IdentityServer.Admin.EntityFramework.MySql.csproj", "src/Playground.IdentityServer.Admin.EntityFramework.MySql/"]
RUN dotnet restore "src/Playground.IdentityServer.Admin/Playground.IdentityServer.Admin.csproj"
COPY . .
WORKDIR "/src/src/Playground.IdentityServer.Admin"
RUN dotnet build "Playground.IdentityServer.Admin.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Playground.IdentityServer.Admin.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENV ASPNETCORE_FORWARDEDHEADERS_ENABLED=true
ENTRYPOINT ["dotnet", "Playground.IdentityServer.Admin.dll"]