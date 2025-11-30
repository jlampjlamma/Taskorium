FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["src/Taskorium.TaskService/Taskorium.TaskService.csproj", "src/Taskorium.TaskService/"]
RUN dotnet restore "src/Taskorium.TaskService/Taskorium.TaskService.csproj"
COPY . .
WORKDIR "/src/src/Taskorium.TaskService"
RUN dotnet build "./Taskorium.TaskService.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./Taskorium.TaskService.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Taskorium.TaskService.dll"]
