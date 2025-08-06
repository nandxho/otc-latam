# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Name: OTC LATAM (Order-to-Cash Latin America)

**OTC LATAM** - Comprehensive Order-to-Cash platform for International Latin America operations, integrating MIOSS, SIV, and related systems into a unified cloud-native solution.

## Repository Architecture

This is an enterprise codebase containing multiple International applications organized by region and system:

### Frontend Applications (Angular)
- **MX-LATAM-MIOSS-GLOBAL-MIOSS_FR**: Angular 18.x MIOSS frontend with PrimeNG, Leaflet maps, Excel export
- **MX-LATAM-NMX-SIV-AdminCondEspeciales_FR**: Angular 8.x special conditions admin  
- **MX-LATAM-NMX-SIV-AdminRangos_FR**: Angular 6.x discount matrix management

### Backend APIs (.NET)
- **MX-LATAM-MIOSS-GLOBAL-MIOSS_BK**: .NET Core API with layered architecture (Api/Context/Logger/Model)
- **MX-LATAM-NMX-SIV-AdminCondEspeciales_BK**: .NET API for special conditions
- **MX-LATAM-NMX-SIV-AdminRangos_BK**: Complex .NET API with business logic layers for discount management

### Legacy Systems (.NET Framework/VB.NET)
- **MX-LATAM-MIOSS-NavistarCmiMIOSS**: Comprehensive CMI MIOSS system 
- **MX-LATAM-NMX-SIV-SIV**: Large VB.NET sales system (Sistema Integral de Ventas)

### Data Integration
- **MX-LATAM-MIOSS-LocUnidad_Exportacion**: SSIS package for unit location export

## Common Development Commands

### Angular Frontend Projects

#### MIOSS Frontend (Angular 18)
```bash
cd MX-LATAM-MIOSS-GLOBAL-MIOSS_FR
npm install
npm start                    # Development server
npm run build-dev           # Development build with /MIOSSFront/ base-href
npm run build-prod          # Production build with /MIOSSFront/ base-href  
npm run build-57            # Environment 57 build
npm test                    # Run tests
```

#### Admin Rangos Frontend (Angular 6)
```bash
cd MX-LATAM-NMX-SIV-AdminRangos_FR/MatrizDeDescuentos
npm install
npm start                    # Development server
npm run build-prod          # Production build with /MatrizDescuentosWeb/ base-href
npm run build-preprod       # Pre-production build
npm run build-qa            # QA build
npm run lint                # Lint code
npm test                    # Run tests
```

### .NET Backend Projects

#### Build Solutions
```bash
# For .NET Core projects
dotnet build
dotnet run

# For .NET Framework projects (use Visual Studio or MSBuild)
msbuild Navistar.sln /p:Configuration=Release
msbuild "International.Aplicaciones.Api.sln" /p:Configuration=Release
```

#### Entity Framework Commands
Most projects use Entity Framework with EDMX files (Database-First approach). For code changes:
- Update EDMX models in Visual Studio
- Regenerate T4 templates
- Check connection strings in Web.config/appsettings.json

## Project Structure Patterns

### Layered Architecture (Common across .NET projects)
- **API/Web Layer**: Controllers, Web Forms, APIs
- **Service Layer**: Business logic and processors  
- **Data Access Layer**: Repositories, DAOs, Entity Framework contexts
- **Domain/Model Layer**: Entities, DTOs, business models
- **Cross-cutting**: Logging, utilities, common libraries

### Key Libraries and Frameworks
- **Frontend**: Angular (6.x to 18.x), PrimeNG, Angular Material, Leaflet, ExcelJS
- **Backend**: Entity Framework (6.x and Core), ASP.NET (Framework and Core)  
- **Database**: SQL Server with multiple databases (Navimex_Mioss, ListaPrecios, latam2, etc.)
- **Integration**: SSIS packages, Web Services (ASMX/WCF), REST APIs

## Configuration Management

### Environment-Specific Configurations
Projects use multiple configuration files for different environments:
- `Web.config` / `Web.config.prepro` / `Web.config.prod` (Legacy .NET)
- `appsettings.json` / `appsettings.Development.json` (Modern .NET)
- Angular environment files with base-href configurations

### Database Connections  
- Multiple Entity Framework contexts per project (e.g., Latam2DBContext, MiossDBContext)
- Centralized connection management using Mastercon.dll
- Connection strings configured per environment

## Testing and Quality

### Testing Commands
```bash
# Angular projects
npm test                    # Run unit tests with Karma/Jasmine
npm run e2e                # Run e2e tests (where available)

# .NET projects - use Visual Studio Test Runner or:
dotnet test                # For .NET Core projects
```

### Code Quality
- TSLint for Angular projects (legacy projects use older versions)
- No specific linting commands found for .NET projects - use Visual Studio code analysis

## Database and Entity Framework

### Database Projects
- Multiple SQL Server databases with dedicated projects
- SSIS packages for data integration
- Extensive use of stored procedures and views

### Entity Framework Patterns
- Database-first approach with EDMX files
- Multiple DbContexts per application
- Repository pattern implementation
- Generic DAO patterns with specialized implementations

## Deployment and CI/CD

### Build Configurations
- Azure DevOps pipelines configured (see azure-pipelines.yml files)
- Environment-specific builds for dev/preprod/prod/qa
- IIS deployment for web applications

### Key Deployment Notes
- Frontend applications deploy with specific base-href paths
- Multiple configuration transformations for different environments
- Database schema managed through Entity Framework migrations or SQL scripts

## Working with This Codebase

### Before Making Changes
1. Identify the correct project based on functionality (MIOSS vs SIV vs Admin tools)
2. Check if working with frontend (Angular) or backend (.NET) components
3. Verify the Angular version and .NET framework version for the specific project
4. Review existing patterns and architectural decisions within that project

### Common Development Tasks
- Frontend changes: Update Angular components, services, and routing
- Backend changes: Modify controllers, services, and data access layers  
- Database changes: Update Entity Framework models and regenerate code
- Configuration: Update environment-specific config files
- Integration: Work with existing web services and API endpoints

### Important Considerations
- Mixed technology stack requires different approaches per project
- Legacy VB.NET code in SIV system requires Visual Studio .NET Framework
- Modern Angular projects use different versions (6.x to 18.x)  
- Database-first Entity Framework approach means schema changes require careful coordination
- Multiple environments with specific build configurations and deployment paths

## Additional Notes

### Coding Guidelines
- **Documento Links**: 
  - siempre muestrame la liga de los documentos que generes