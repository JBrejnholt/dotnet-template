#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["${{values.artifact_id}}/${{values.artifact_id}}.csproj", "${{values.artifact_id}}/"]
RUN dotnet restore "${{values.artifact_id}}/${{values.artifact_id}}.csproj"
COPY . .
WORKDIR "/src/${{values.artifact_id}}"
RUN dotnet build "${{values.artifact_id}}.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "${{values.artifact_id}}.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENV ASPNETCORE_URLS http://+:8080
ENTRYPOINT ["dotnet", "${{values.artifact_id}}.dll"]
