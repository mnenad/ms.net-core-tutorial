In the next step, we will modify the middleware so it can run on the cloud as well.

Modify the dependencies section in project.json to look like this:

  "dependencies": {
        ...
        "Microsoft.Extensions.Configuration.CommandLine": "1.0.0"
        ...
  },

  Add following line at the begining of the main method of the Program.cs class:

  var config = new ConfigurationBuilder()
                .AddCommandLine(args)
            	.Build();
  
  Modify var host line to look like this:

  var host = new WebHostBuilder()
                    .UseKestrel()
                    .UseStartup<Startup>()
                    .UseConfiguration(config)
                    .Build();

  After all is done the class should look like this:

    using System;
    using Microsoft.AspNetCore.Hosting;
    using Microsoft.AspNetCore.Builder;
    using Microsoft.Extensions.Configuration;

    namespace WebApplication.hw
    {
        public class Program
        {
            public static void Main(string[] args)
            {
                var config = new ConfigurationBuilder()
                    .AddCommandLine(args)
                    .Build();

                var host = new WebHostBuilder()
                    .UseKestrel()
                    .UseStartup<Startup>()
                    .UseConfiguration(config)
                    .Build();

                host.Run();
            }
        }
    }
 
<walkthrough files created>
dotnet restore
<pulling down dependencies>
dotnet build
dotnet run

Go to your browser and hit http://localhost:5000 to make sure everything is still running properly

Go to the directory where the Program.cs and project.json files are and push to the cloud

(make sure that cloud buildpack supports your version of .net core)

Pushing .net core source code

cf push <application_name> 

Pushing published version of the app

dotnet publish -f netcoreapp1.1 -r ubuntu.14.04-x64 -o $cd\publish

create manifest.yml file:
---
applications:
- name: HelloWorld
  random-route: true
  memory: 512M
  buildpack: https://github.com/cloudfoundry-community/asp.net5-buildpack.git
  env:
    ASPNETCORE_ENVIRONMENT: production

cf push -f manifest.yml -p publish

