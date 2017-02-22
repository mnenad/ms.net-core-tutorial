In step two, we add middleware to the application that responds with "Hello World" to all HTTP requests. This illustrates how easy it is to go from a console app to a hello world microservice, as well as the use of basic middleware.

Modify the dependencies section in project.json to look like this:

  "dependencies": {
        "Microsoft.AspNetCore.Server.Kestrel": "1.0.0"
  },

  Add following using statement at the top of the Program.cs class:

  using Microsoft.AspNetCore.Hosting;
  using Microsoft.AspNetCore.Builder;
  using Microsoft.Extensions.Configuration;

  Replace namespace line with the following:

  namespace WebApplication.hw

  Replace Console.WriteLine("Hello World!"); with the folloiwng:

  var host = new WebHostBuilder()
 	     .UseKestrel()
 		 .UseStartup<Startup>()
 	     .Build();
  host.Run();
  
  The class should look like the picture bellow:

  <todo>    Import picture:lab2-project-class

Add new Startup.cs class like this:

using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Logging;
using Microsoft.AspNetCore.Http;

namespace WebApplication.hw{
    public class Startup
    {
        public void Configure(IApplicationBuilder app)
        {
            app.Run(async (context) =>
            {
                await context.Response.WriteAsync("Hello, world!\n");
            });
        }
    }
}

<walkthrough files created>
dotnet restore
<pulling down dependencies>
dotnet build
dotnet run

Go to your browser and hit http://localhost:5000
