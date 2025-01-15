# Use the official .NET 6.0 SDK image as the base image
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build

# Set the working directory inside the container
WORKDIR /app

# Copy the project file and restore any dependencies (via `dotnet restore`)
COPY *.csproj ./
RUN dotnet restore

# Copy the rest of the application code
COPY . ./

# Build the application
RUN dotnet publish -c Release -o out

# Use the official .NET 6.0 runtime image to run the application
FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS runtime

# Set the working directory inside the container
WORKDIR /app

# Copy the published application from the build stage
COPY --from=build /app/out ./

# Expose the application port (adjust if necessary)
EXPOSE 80

# Start the application
ENTRYPOINT ["dotnet", "Cervantes.Web.dll"]
