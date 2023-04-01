#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80


FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["PhysicianAvailability_Service/PhysicianAvailability_Service.csproj", "PhysicianAvailability_Service/"]
COPY ["PhysicianAvailability_BussinessLogic/PhysicianAvailability_BussinessLogic.csproj", "PhysicianAvailability_BussinessLogic/"]
COPY ["PhysicianAvailability_FluentAPI/PhysicianAvailability_FluentAPI.csproj", "PhysicianAvailability_FluentAPI/"]
COPY ["PhysicianAvailability_Models/PhysicianAvailability_Models.csproj", "PhysicianAvailability_Models/"]
RUN dotnet restore "PhysicianAvailability_Service/PhysicianAvailability_Service.csproj"
COPY . .
WORKDIR "/src/PhysicianAvailability_Service"
RUN dotnet build "PhysicianAvailability_Service.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "PhysicianAvailability_Service.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "PhysicianAvailability_Service.dll"]
