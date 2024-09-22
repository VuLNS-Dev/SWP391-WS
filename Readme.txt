INSTRUCTIONS

===============================================================================
1. SETUP
===============================================================================
1.1. Download & install EF Core Power Tools
https://marketplace.visualstudio.com/items?itemName=ErikEJ.EFCorePowerTools
Note: Closes Visual Studio & MSSQL Server Management tool before installing

1.2. Opens MSSQL Server Management tool and executes SWP391ProductManagementSystem.sql script


===============================================================================
2. SOME UTILITY COMMANDS & SOURCE CODE
===============================================================================

dotnet add package Microsoft.EntityFrameworkCore --version 8.0.5
dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 8.0.5
dotnet add package Microsoft.EntityFrameworkCore.Tools --version 8.0.5
dotnet add package Microsoft.Extensions.Configuration --version 8.0.0
dotnet add package Microsoft.Extensions.Configuration.Json --version 8.0.0

-------------------------------------------------------------------------------

### dotnet tool install --global dotnet-ef --version 8.0.8
dotnet ef dbcontext scaffold "Data Source=VULNS-SPACE;Initial Catalog=SWP391ProductManagementSystem;Persist Security Info=True;User ID=vulns;Password=*********;Encrypt=False" Microsoft.EntityFrameworkCore.SqlServer --output-dir Models

-------------------------------------------------------------------------------

public static string GetConnectionString(string connectionStringName)
{
    var config = new ConfigurationBuilder()
        .SetBasePath(AppDomain.CurrentDomain.BaseDirectory)
        .AddJsonFile("appsettings.json")
        .Build();

    string connectionString = config.GetConnectionString(connectionStringName);
    return connectionString;
}
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder.UseSqlServer(GetConnectionString("DefaultConnection"));

-------------------------------------------------------------------------------

{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "AllowedHosts": "*",
  "ConnectionStrings": {
    "DefaultConnection": "Data Source=VULNS-SPACE;Initial Catalog=eFastBanking;Persist Security Info=True;User ID=vulns;Password=*********;Encrypt=False"
  }
}

-------------------------------------------------------------------------------

builder.Services.AddScoped<UnitOfWork>();
builder.Services.AddControllers().AddJsonOptions(options => options.JsonSerializerOptions.ReferenceHandler = ReferenceHandler.IgnoreCycles);
