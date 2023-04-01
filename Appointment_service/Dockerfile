#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80


FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["Appointment_service/Appointment_service.csproj", "Appointment_service/"]
COPY ["Appointment_BusinessLogic/Appointment_BusinessLogic.csproj", "Appointment_BusinessLogic/"]
COPY ["Appointment_FluentApi/Appointment_FluentApi.csproj", "Appointment_FluentApi/"]
COPY ["Appointment_Models/Appointment_Models.csproj", "Appointment_Models/"]
RUN dotnet restore "Appointment_service/Appointment_service.csproj"
COPY . .
WORKDIR "/src/Appointment_service"
RUN dotnet build "Appointment_service.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Appointment_service.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Appointment_service.dll"]
