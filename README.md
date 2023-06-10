# weekly64

https://www.baeldung.com/ops/docker-compose

https://blog.sixeyed.com/understanding-microsofts-docker-images-for-net-apps/



https://mcr.microsoft.com/en-us/product/dotnet/sdk/about



Create dotnet webapi



https://github.com/dotnet/dotnet-docker/blob/main/samples/build-in-sdk-container.md

debian@debian:~/projects$ docker run --rm -v $(pwd)/app1:/app -w /app mcr.microsoft.com/dotnet/sdk:7.0 dotnet new webapi



Build dotnet webapi



debian@debian:~/projects$ docker run --rm -v $(pwd)/app2:/app -w /app mcr.microsoft.com/dotnet/sdk:5.0 dotnet publish -c Release -o out



test dotnet webapi



run dotnet webapi

https://github.com/dotnet/dotnet-docker/blob/main/samples/run-in-sdk-container.md

debian@debian:~/projects$ docker run --rm -it -p 8008:80 -v $(pwd)/app2:/app/ -w /app -e ASPNETCORE_URLS=http://+:80 -e ASPNETCORE_ENVIRONMENT=Development mcr.microsoft.com/dotnet/sdk:5.0 dotnet run --no-launch-profile

http://localhost:8008/swagger/index.html



http://localhost:8008/WeatherForecast

debian@debian:~/projects/app$ docker run --rm -v $(pwd)/app1:/app -w /app mcr.microsoft.com/dotnet/sdk:5.0 dotnet new --list

debian@debian:~/projects/app$ docker run --rm -v $(pwd)/app3:/app -w /app mcr.microsoft.com/dotnet/sdk:5.0 dotnet new web

https://github.com/maximilianou/doc24_compose


```
version: '3.7'
services:
  front_srv:
    container_name: front_srv
    hostname: frontend
    image: mcr.microsoft.com/dotnet/sdk:5.0
    command:
      - dotnet
      - run
      - --no-launch-profile
      - --project
      - /app
    volumes:
      - './app3:/app'
    ports:
      - '8030:80'
    environment:
      - ASPNETCORE_URLS=http://+:80
      - ASPNETCORE_ENVIRONMENT=Development
    networks:
      - net_intranet
  back_srv:
    container_name: back_srv
    hostname: backtend
    image: mcr.microsoft.com/dotnet/sdk:5.0
    command:
      - dotnet
      - run
      - --no-launch-profile
      - --project
      - /app
    volumes:
      - './app2:/app'
    ports:
      - '8020:80'
    environment:
      - ASPNETCORE_URLS=http://+:80
      - ASPNETCORE_ENVIRONMENT=Development
    networks:
      - net_intranet
networks:
  net_intranet:

`using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;

namespace app
{
public class Startup
{
// This method gets called by the runtime. Use this method to add services to the container.
// For more information on how to configure your application, visit https://go.microsoft.com/fwlink/?LinkID=398940
public void ConfigureServices(IServiceCollection services)
{
}

    // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }

        app.UseRouting();

        app.UseDefaultFiles();
        app.UseStaticFiles();
        app.UseFileServer();

        app.UseEndpoints(endpoints =>
        {
            endpoints.MapGet("/", async context =>
            {
                await context.Response.WriteAsync("Hello World!");
            });
        });
    }
}

}
`

`using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Hosting;
using Microsoft.Extensions.Logging;

namespace app
{
public class Program
{
public static void Main(string[] args)
{
CreateHostBuilder(args).Build().Run();
}

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStartup<Startup>();
            });
}

}
`

app3/wwwroot/front.hml
<!DOCTYPE html> 
<html>     
<head><title>Reggio</title></head>     
<body>Reggio</body> 
</html>
```