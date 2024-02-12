Crear proyecto de ejemplo
    dotnet new console -o App -n DotNet.Docker
publicar aplicación
    dotnet publish name
Crear archivo Dockerfile
    FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build-env
    WORKDIR /App

    # Copy everything
    COPY . ./
    # Restore as distinct layers
    RUN dotnet restore
    # Build and publish a release
    RUN dotnet publish -c Release -o out

    # Build runtime image
    FROM mcr.microsoft.com/dotnet/aspnet:8.0
    WORKDIR /App
    COPY --from=build-env /App/out .
    ENTRYPOINT ["dotnet", "DotNet.Docker.dll"]

Crear imagen
    docker build -t counter-image -f Dockerfile .

Crear contenedor
    docker create --name core-counter counter-image

Ver conntenedores
    docker ps -a
Arrancar contenedor
    docker start name
parar contenedor
    docker stop name

Conectarse
    docker attach --sig-proxy=false core-counter
 El parámetro --sig-proxy=false garantiza que Ctrl+C no va a detener el proceso en el contenedor.

 Eliminar contenedor
 docker rm name

