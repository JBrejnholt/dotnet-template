FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build-env
WORKDIR /Example

# Copy everything
COPY . ./
# Restore as distinct layers
RUN dotnet restore
# Build and publish a release
RUN dotnet publish -c Release -o out

# Build runtime image
FROM mcr.microsoft.com/dotnet/aspnet:7.0
WORKDIR /Example
COPY --from=build-env /Example/out .
ENV ASPNETCORE_URLS http://+:8080;http://+:8443
ENTRYPOINT ["dotnet", "Example.dll"]