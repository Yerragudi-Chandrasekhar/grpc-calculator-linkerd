#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM docker.io/bitnami/aspnet-core AS base
WORKDIR /app

FROM docker.io/bitnami/aspnet-core AS build
WORKDIR /src
COPY ["MathFanboy/MathFanboy.csproj", "MathFanboy/"]
RUN dotnet restore "MathFanboy/MathFanboy.csproj"
COPY . .
WORKDIR "/src/MathFanboy"
RUN dotnet build "MathFanboy.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "MathFanboy.csproj" -c Release -o /app/publish

FROM base AS final
ENV AdditionEndpoint http://addition:5001
ENV DivisionEndpoint http://division:5002
ENV MultiplicationEndpoint http://multiplication:5003
ENV SubstractionEndpoint http://substraction:5004
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "MathFanboy.dll"]
