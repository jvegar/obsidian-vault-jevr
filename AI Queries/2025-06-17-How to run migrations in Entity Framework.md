# Running Entity Framework Core Migrations in .NET Core

To run migrations with Entity Framework Core in a .NET Core application, follow these steps:

## Prerequisites
1. Install the Entity Framework Core tools (if not already installed):
   ```sh
   dotnet tool install --global dotnet-ef
   ```

2. Ensure you have these NuGet packages in your project:
   - `Microsoft.EntityFrameworkCore`
   - `Microsoft.EntityFrameworkCore.Design`
   - The appropriate database provider (e.g., `Microsoft.EntityFrameworkCore.SqlServer`)

## Basic Migration Commands

### 1. Create a migration
```sh
dotnet ef migrations add InitialCreate
```
This creates a new migration with the name "InitialCreate" (you can use any descriptive name).

### 2. Apply migrations to database
```sh
dotnet ef database update
```
This applies all pending migrations to the database.

### 3. Remove the last migration (if needed)
```sh
dotnet ef migrations remove
```

## Advanced Usage

### Apply a specific migration
```sh
dotnet ef database update MigrationName
```

### Generate SQL script (without applying it)
```sh
dotnet ef migrations script -o migration.sql
```

### For a specific DbContext (when you have multiple contexts)
```sh
dotnet ef migrations add InitialCreate --context YourDbContext
dotnet ef database update --context YourDbContext
```

## Running Migrations Programmatically

You can also apply migrations from your application code:

```csharp
public static void Main(string[] args)
{
    var host = CreateHostBuilder(args).Build();
    
    using (var scope = host.Services.CreateScope())
    {
        var db = scope.ServiceProvider.GetRequiredService<YourDbContext>();
        db.Database.Migrate();
    }
    
    host.Run();
}
```

## Troubleshooting

- If you get "No executable found matching command 'dotnet-ef'":
  - Make sure you installed the tools (`dotnet tool install --global dotnet-ef`)
  - Or use `dotnet ef` from within your project directory with the local tools version

- For design-time operations (like migrations), your DbContext needs a public parameterless constructor or a design-time factory.

Would you like more specific information about any of these steps?