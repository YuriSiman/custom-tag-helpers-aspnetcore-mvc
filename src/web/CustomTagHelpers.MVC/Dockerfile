#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0-buster-slim AS build
WORKDIR /src
COPY ["src/web/CustomTagHelpers.MVC/CustomTagHelpers.MVC.csproj", "src/web/CustomTagHelpers.MVC/"]
RUN dotnet restore "src/web/CustomTagHelpers.MVC/CustomTagHelpers.MVC.csproj"
COPY . .
WORKDIR "/src/src/web/CustomTagHelpers.MVC"
RUN dotnet build "CustomTagHelpers.MVC.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "CustomTagHelpers.MVC.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "CustomTagHelpers.MVC.dll"]