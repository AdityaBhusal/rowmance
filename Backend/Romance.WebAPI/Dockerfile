# Stage 1: Base runtime image
FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS base
WORKDIR /app
EXPOSE 8080

# Stage 2: Build environment
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
WORKDIR /src

# Step 2a: Copy all .csproj files first (matching your exact spelling)
COPY ["Rowmance.WebAPI/Rowmance.WebAPI.csproj", "Rowmance.WebAPI/"]
COPY ["Rowmance.Domain/Rowmance.Domain.csproj", "Rowmance.Domain/"]
COPY ["Rowmance.Application/Rowmance.Application.csproj", "Rowmance.Application/"]
COPY ["Rowmance.Infrastructure/Rowmance.Infrastructure.csproj", "Rowmance.Infrastructure/"]

# Step 2b: Restore dependencies for the main API project
RUN dotnet restore "Romance.WebAPI/Romance.WebAPI.csproj"

# Step 2c: Copy the rest of your actual source code
COPY . .

# Step 2d: Build the application
WORKDIR "/src/Romance.WebAPI"
RUN dotnet build "Romance.WebAPI.csproj" -c Release -o /app/build

# Stage 3: Publish the app
FROM build AS publish
RUN dotnet publish "Romance.WebAPI.csproj" -c Release -o /app/publish /p:UseAppHost=false

# Stage 4: Final production image
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Romance.WebAPI.dll"]