# CLAUDE.md - OTC LATAM Project Context v2.0

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Name: OTC LATAM (Order-to-Cash Latin America)

**OTC LATAM** - Modernization initiative to consolidate and unify two primary truck sales processes of International Trucks brand that are currently supported by separate legacy applications in `/mnt/c/victor/trabajo/apps/azure-repos/`:

1. **Domestic Sales Process** - Supported by MIOSS system
2. **Export Sales Process** - Supported by SIV (Sistema Integral de Ventas) system

These two business processes have been unified operationally, and the next step is to **unify and modernize the technology platform** supporting them into a single, cloud-native OTC LATAM solution.

## Current Technology Stack Analysis (Based on Repository Review)

### MIOSS System (Domestic/Export Sales)
- **Frontend**: Angular 18.2 with PrimeNG 17.x (MODERN)
- **Backend**: .NET Core 2.1 API with layered architecture (NEEDS UPGRADE)
- **Legacy Core**: Comprehensive .NET Framework/VB.NET system (REQUIRES MODERNIZATION)
- **Databases**: SQL Server with multiple contexts (Navimex_Mioss, ListaPrecios, Latam2, etc.)

### SIV System (Sistema Integral de Ventas)
- **AdminRangos Frontend**: Angular 6.1 with PrimeNG 6.x (LEGACY - discount matrix management)
- **AdminCondEspeciales Frontend**: Angular 8.2 with PrimeNG 8.x (OUTDATED - special conditions)
- **Backend APIs**: .NET Core 2.1 with complex business logic (18+ projects for discount management)
- **Price List Management**: Angular 6.0 frontend with .NET Core backend (LEGACY)

### Critical Technical Debt
- **.NET Core 2.1**: End-of-life, security vulnerabilities
- **Angular 6.x/8.x**: Multiple major versions behind
- **Mixed architecture patterns**: Some modern, some legacy
- **Security vulnerabilities**: Multiple outdated dependencies
- **Legacy VB.NET components**: Require complete modernization

## Target Architecture (Based on ARQUITECTURA_REVISADA_100_USUARIOS.md)

**Recommended Approach: Monolito Modular (Modular Monolith)** - NOT microservices due to scale (100 users)

### Unified Platform Structure
```
OTC-LATAM-Unified/
├── Modules/
│   ├── Sales/          (MIOSS functionality)
│   ├── Pricing/        (Price lists & discounts)
│   ├── Customers/      (Customer management)
│   ├── Orders/         (Order management)
│   ├── Discounts/      (SIV discount matrix)
│   ├── SpecialConditions/ (SIV special conditions)
│   └── Reporting/      (Unified reporting)
├── Shared/
│   ├── Infrastructure/
│   ├── Domain/
│   └── Application/
└── WebAPI/
```

### Technology Stack Goals
- **Frontend**: Angular 18+ with feature modules (unified UI)
- **Backend**: .NET 8 modular monolith with Clean Architecture
- **Database**: SQL Server with unified schema design
- **Infrastructure**: Azure Container Apps (already templated in repository)

## Repository Architecture

This is an enterprise codebase containing multiple International Trucks applications organized by region and system:

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

## Business Context - Truck Sales Processes

### MIOSS System - Domestic & Export Sales
**Business Process**: Complete order-to-cash cycle for International Trucks
- **Order Management**: Complex approval workflows and authorization engine
- **Pricing Integration**: Dynamic pricing with transfer price calculations  
- **Document Generation**: Automated PDF/Excel generation for orders
- **Logistics Integration**: Maps and shipping coordination via Leaflet
- **Multi-market Support**: Domestic and export sales in same platform

### SIV System - Sales Support Functions  
**Business Process**: Specialized sales administration and pricing management
- **Discount Matrix Management**: Complex discount criteria and multi-level authorization
- **Special Conditions Admin**: Special pricing and terms management
- **Price List Management**: Centralized pricing with approval workflows
- **User Role Management**: Fine-grained permission system

### Business Unification Drivers
1. **Process Consolidation**: Sales teams now handle both domestic and export
2. **Customer Experience**: Single interface for all truck sales processes
3. **Operational Efficiency**: Eliminate duplicate data entry and processes
4. **Reporting Unity**: Consolidated sales analytics and KPIs
5. **Technology Simplification**: Reduce maintenance overhead of multiple systems

## Common Development Commands

### Angular Frontend Projects

#### MIOSS Frontend (Angular 18) - MODERN
```bash
cd MX-LATAM-MIOSS-GLOBAL-MIOSS_FR
npm install
npm start                    # Development server
npm run build-dev           # Development build with /MIOSSFront/ base-href
npm run build-prod          # Production build with /MIOSSFront/ base-href  
npm run build-57            # Environment 57 build
npm test                    # Run tests
```

#### SIV Admin Rangos Frontend (Angular 6) - LEGACY
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

#### SIV Admin Condiciones Especiales (Angular 8) - OUTDATED  
```bash
cd MX-LATAM-NMX-SIV-AdminCondEspeciales_FR
npm install
npm start                    # Development server
npm run build              # Production build
npm test                   # Run tests
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

## Current vs Target Architecture Analysis

### Current State: Fragmented Systems
- **Multiple Technology Stacks**: Angular 6/8/18, .NET Core 2.1, .NET Framework
- **Duplicated Business Logic**: Similar functionality across MIOSS and SIV
- **Inconsistent UI/UX**: Different design systems and user experiences  
- **Complex Integration**: Multiple APIs and data sources
- **High Maintenance Cost**: Multiple frameworks to maintain and secure

### Target State: Unified Modular Monolith
- **Single Technology Stack**: Angular 18 + .NET 8 + SQL Server
- **Consolidated Business Logic**: Shared modules for common functionality
- **Unified UI/UX**: Single design system based on PrimeNG
- **Simplified Integration**: Single API with modular endpoints
- **Reduced Complexity**: One application to deploy, monitor, and maintain

## Project Structure Patterns

### Current Layered Architecture (Common across .NET projects)
- **API/Web Layer**: Controllers, Web Forms, APIs
- **Service Layer**: Business logic and processors  
- **Data Access Layer**: Repositories, DAOs, Entity Framework contexts
- **Domain/Model Layer**: Entities, DTOs, business models
- **Cross-cutting**: Logging, utilities, common libraries

### Target Modular Monolith Structure  
- **WebAPI Layer**: Unified controllers with feature-based organization
- **Application Layer**: CQRS with feature modules (Sales, Pricing, etc.)
- **Domain Layer**: Domain entities and business rules by bounded context
- **Infrastructure Layer**: Data access, external services, caching
- **Shared Kernel**: Common utilities, cross-cutting concerns

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

## Modernization Strategy & Working with This Codebase

### Modernization Approach: Gradual Migration
1. **Security First**: Update all packages with security vulnerabilities
2. **Framework Upgrades**: Migrate .NET Core 2.1 → .NET 8, Angular 6/8 → Angular 18
3. **Architecture Consolidation**: Merge similar functionality from MIOSS and SIV
4. **UI/UX Unification**: Create consistent user experience across all functions
5. **Database Integration**: Consolidate schemas and eliminate redundancy

### Before Making Changes
1. **Identify System Context**: 
   - MIOSS (modern Angular 18, legacy .NET Core 2.1)
   - SIV AdminRangos (legacy Angular 6, complex business logic)
   - SIV AdminCondEspeciales (outdated Angular 8)
   - Price Lists (legacy Angular 6)
2. **Check Technology Version**: Angular version and .NET framework version
3. **Review Business Impact**: Changes may affect both domestic and export sales
4. **Consider Migration Path**: How changes fit into unification strategy

### Priority Development Tasks
1. **Immediate Security Fixes**:
   - Update vulnerable packages in all Angular applications
   - Upgrade .NET Core 2.1 applications to .NET 8
   - Replace deprecated authentication mechanisms

2. **Frontend Modernization**:
   - Migrate Angular 6/8 applications to Angular 18
   - Standardize on PrimeNG 17+ for consistent UI
   - Implement Angular feature modules for modularity

3. **Backend Consolidation**:
   - Merge similar controllers and services across MIOSS/SIV
   - Implement Clean Architecture patterns
   - Create shared business logic modules

4. **Database Optimization**:
   - Consolidate similar tables/schemas across systems
   - Optimize Entity Framework queries and contexts
   - Implement proper indexing and performance monitoring

### Migration Considerations
- **Business Continuity**: Systems handle critical truck sales processes
- **Data Integrity**: Multiple databases with complex relationships  
- **User Training**: UI/UX changes will require user adaptation
- **Legacy VB.NET**: NavistarCmiMIOSS system requires complete rewrite
- **Integration Points**: External systems may depend on current APIs
- **Deployment Strategy**: Blue-green deployment for zero downtime

### Current Technical Debt Priority
1. **Critical**: .NET Core 2.1 end-of-life security issues
2. **High**: Angular 6.x/8.x security vulnerabilities  
3. **Medium**: Architecture inconsistencies and code duplication
4. **Low**: Performance optimizations and developer experience improvements

## Additional Notes

### Coding Guidelines
- **Documento Links**: 
  - siempre muestrame la liga de los documentos que generes