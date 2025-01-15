FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY *.csproj ./
RUN dotnet restore
COPY . .
WORKDIR "/src/Cervantes.Web"
RUN dotnet build "Cervantes.Web.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Cervantes.Web.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Cervantes.Web.dll"]

# update packages
RUN apt-get update -y && apt-get upgrade -y && apt-get autoremove -y
# create new user with lower privileges
RUN useradd -ms /bin/bash cervantes
# change permissions of particular directories
RUN chown -R cervantes /app/wwwroot

# switch to the newly created user
USER cervantes
