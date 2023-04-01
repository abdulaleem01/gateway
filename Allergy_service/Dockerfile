#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80


FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["Allergy_service/Allergy_service.csproj", "Allergy_service/"]
COPY ["Allergy_Business_Logic/Allergy_Business_Logic.csproj", "Allergy_Business_Logic/"]
COPY ["Allergy_EntityApi/Allergy_EntityApi.csproj", "Allergy_EntityApi/"]
COPY ["Allergy_Models/Allergy_Models.csproj", "Allergy_Models/"]
RUN dotnet restore "Allergy_service/Allergy_service.csproj"
COPY . .
WORKDIR "/src/Allergy_service"
RUN dotnet build "Allergy_service.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Allergy_service.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Allergy_service.dll"]
