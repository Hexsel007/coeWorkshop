# LAB Module 01 - Lesson 02



## Lesson 02 – Docker | M01: Containers and Kubernetes



### Task 01 - Open PwC LAB

| Task | Name                                                |
| ---- | --------------------------------------------------- |
| 1    | Access Azure Lab Services - https://labs.azure.com/ |
| 2    | Sign in  with your PwC Account @pwc.com             |
| 3    | In My Virtual Machines, Start your VM               |
| 4.   | Connect in your VM                                  |
| 5    | Passord: PwC@2021                                   |



### Task 02 - Docker Commands

Run all commands in PowerShell or Shell

#### How to list images

```console
docker images
```



#### How to Download Image

```dockerfile
docker pull mcr.microsoft.com/mssql/server:2019-latest 
```



#### How to Run Image 

```dockerfile
docker run -d -p 8080:80 docker/getting-started
```

```dockerfile
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=SqlContainer@123" -p 1433:1433 --name sql1 -h sql1 -d mcr.microsoft.com/mssql/server:2019-latest 
```



#### How to remove an image

```code
docker rmi <<IMAGE>>
```



#### How to view available containers

```code
docker ps
```

```code
docker ps -a
```



#### How to Exec Commands containers

```code
docker exec -it sql1 "bash"
```

```code
/opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P "SqlContainer@123"

```



#### How to pause a container

```dockerfile
docker pause <<container>>

```

Pausing a container will suspend all processes. This command enables the container to continue processes at a later stage. The `docker unpause` command un-suspends all processes in the specified containers. 



#### How to restart a container

To restart containers, run the `docker restart` command. Here is an example.

```console
docker restart <<container>>

```

The container receives a stop command, followed by a start command. If the container doesn't respond to the stop command, then a kill signal is sent.



#### How to stop a container

To stop a running container, run the `docker stop` command. Here is an example.

```console
docker stop <<container>>

```

The stop command sends a termination signal to the container and the process running in the container.



#### How to remove a container

To remove a container, run the `docker rm` command. Here is an example.

```console
docker rm <<container>>

```

After you remove the container, all data in the container is destroyed. It's essential to always consider containers as temporary when thinking about storing data.



### Task 3 - Run Application Docker File



1. In the Azure Cloud Shell in the portal, run the following command to download the source code for the sample web app. This web app is simple. It presents a single page that contains static text and a carousel control that rotates through a series of images.

```bash
git clone https://github.com/MicrosoftDocs/mslearn-deploy-run-container-app-service.git

```



2. Move to the source folder.

```bash
cd mslearn-deploy-run-container-app-service/dotnet

```

 

3. Read Content of DockerFile 

```dockerfile
FROM microsoft/dotnet:2.1-aspnetcore-runtime AS base
WORKDIR /app
EXPOSE 80

FROM microsoft/dotnet:2.1-sdk AS build
WORKDIR /src
COPY ["SampleWeb/SampleWeb.csproj", "SampleWeb/"]
RUN dotnet restore "SampleWeb/SampleWeb.csproj"
COPY . .
WORKDIR "/src/SampleWeb"
RUN dotnet build "SampleWeb.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "SampleWeb.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "SampleWeb.dll"]

```

 

4. Build your Own Image

```bash
docker build -t workshop/my-webwerver .

```

 

4. Run your Image

```bash
docker run  -p 8081:80 --name Webserver1 -d workshop/my-webwerver

```

 
