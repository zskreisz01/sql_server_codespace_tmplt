# SQL Server 2025 Docker Setup

This project successfully sets up SQL Server 2025 in GitHub Codespaces using Docker Compose with the official Microsoft image.

## Project Structure

### Docker Setup
- [docker-compose.yml](docker-compose.yml) - Docker Compose configuration
- [Dockerfile](Dockerfile) - Custom Dockerfile based on official SQL Server 2025 image
- [test-queries.sql](test-queries.sql) - Sample SQL queries for testing

### SQL Database Project
- [SampleDatabase/](SampleDatabase/) - SQL Server Database Project (.sqlproj)
  - [Tables/](SampleDatabase/Tables/) - Table definitions (Customers, Orders, Products)
  - [StoredProcedures/](SampleDatabase/StoredProcedures/) - Stored procedures
  - [Views/](SampleDatabase/Views/) - Database views
  - [DEPLOYMENT.md](SampleDatabase/DEPLOYMENT.md) - Complete deployment guide
  - [SampleDatabase.sqlproj](SampleDatabase/SampleDatabase.sqlproj) - Project file
  - `bin/Debug/SampleDatabase.dacpac` - Generated dacpac file (after build)

## Quick Start

1. Start SQL Server container:
```bash
docker-compose up -d
```

2. Check container status:
```bash
docker ps
```

3. View logs:
```bash
docker logs sqlserver2025
```

4. Run a query:
```bash
docker exec sqlserver2025 /opt/mssql-tools18/bin/sqlcmd -S localhost -U SA -P 'YourStrong@Passw0rd' -C -Q "SELECT @@VERSION;"
```

## Connection Details

- **Host**: localhost
- **Port**: 1433
- **Username**: SA
- **Password**: YourStrong@Passw0rd
- **Edition**: Enterprise Developer Edition (64-bit)
- **Version**: SQL Server 2025 (RTM) - 17.0.1000.7

## Stopping the Container

```bash
docker-compose down
```

To remove volumes as well:
```bash
docker-compose down -v
```

## SQL Database Project (DACPAC Deployment)

This project includes a complete SQL Server Database Project with dacpac deployment support.

### Quick Start

1. **Build the project:**
```bash
cd SampleDatabase
dotnet build
```

2. **Deploy using sqlpackage:**
```bash
export PATH="$HOME/.dotnet:$PATH"
export DOTNET_ROOT="$HOME/.dotnet"

sqlpackage /Action:Publish \
  /SourceFile:bin/Debug/SampleDatabase.dacpac \
  /TargetConnectionString:"Server=localhost,1433;Database=SampleDatabase;User Id=SA;Password=YourStrong@Passw0rd;TrustServerCertificate=True;"
```

3. **Verify deployment:**
```bash
docker exec sqlserver2025 /opt/mssql-tools18/bin/sqlcmd -S localhost -U SA -P 'YourStrong@Passw0rd' -d SampleDatabase -C -Q "SELECT * FROM Customers;"
```

### Database Schema

**Tables:**
- `Customers` - Customer information with email and phone
- `Orders` - Customer orders with foreign key relationship
- `Products` - Product catalog with pricing and inventory

**Stored Procedures:**
- `usp_AddCustomer` - Add new customer
- `usp_GetCustomerOrders` - Get all orders for a customer

**Views:**
- `vw_CustomerOrderSummary` - Aggregated customer order statistics

For detailed deployment instructions, see [SampleDatabase/DEPLOYMENT.md](SampleDatabase/DEPLOYMENT.md)

## Official Documentation

Based on Microsoft's official SQL Server 2025 Docker documentation:
- Image: `mcr.microsoft.com/mssql/server:2025-latest`
- [Microsoft Learn - SQL Server on Docker](https://learn.microsoft.com/en-us/sql/linux/quickstart-install-connect-docker)
- [GitHub - mssql-docker](https://github.com/microsoft/mssql-docker)
- [Microsoft Learn - SQL Database Projects](https://learn.microsoft.com/en-us/sql/tools/sql-database-projects/sql-database-projects)
- [Microsoft Learn - Create and Deploy SQL Project](https://learn.microsoft.com/en-us/sql/tools/sql-database-projects/tutorials/create-deploy-sql-project)


