FROM mcr.microsoft.com/dotnet/core/runtime:3.1-buster-slim AS base
WORKDIR /app

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["WorkerL/WorkerL.csproj", "WorkerL/"]
RUN dotnet restore "WorkerL/WorkerL.csproj"
COPY . .
WORKDIR "/src/WorkerL"
RUN dotnet build "WorkerL.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "WorkerL.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WorkerL.dll"]