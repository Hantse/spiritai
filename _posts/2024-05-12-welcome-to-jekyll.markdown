---
layout: post
title:  "Welcome to Jekyll!"
date:   2024-05-12 18:50:14 +0200
categories: jekyll update
---

Le versionnage via le chemin de l'URL, également connu sous le nom de versionnage de chemin, consiste à intégrer la version de l'API directement dans le chemin de l'URL. Cela permet une distinction claire entre les différentes versions de l'API en modifiant l'URL pour inclure un segment spécifique de version, comme `/v1/`, `/v2/`, etc.  

### Avantages
**Clarté et Logique**: Les versions sont explicitement claires dans l'URL, facilitant ainsi aux développeurs et aux utilisateurs finaux de comprendre avec quelle version de l'API ils interagissent. 
- **Isolation**: Chaque version peut être traitée comme une application distincte, ce qui peut simplifier la gestion du déploiement et le contrôle des accès version par version. 
- **Cache Friendly**: Les URLs versionnées séparément sont plus faciles à gérer par les proxys et les caches, car ils sont vus comme des ressources complètement différentes.  

### Inconvénients  
- **Maintenance de l’URL Routing**: Chaque nouvelle version peut nécessiter une mise à jour des chemins dans le système de routage, ce qui peut augmenter la complexité du code. 
- **Risque de Duplication**: Peut entraîner une duplication de code si les changements entre versions ne sont pas bien gérés, notamment si beaucoup de codes sont partagés entre versions.  ### Exemple de mise en œuvre en ASP.NET Core 8  Dans ASP.NET Core 8, le versionnage par chemin de l'URL peut être configuré en utilisant des attributs de routage pour définir explicitement les chemins de version dans les contrôleurs. 

Voici comment cela peut être fait :  
```csharp 
public void ConfigureServices(IServiceCollection services) {     
services.AddControllers();     
services.AddApiVersioning(options =>    
{         
options.ApiVersionReader = new UrlSegmentApiVersionReader();         
options.AssumeDefaultVersionWhenUnspecified = true;         
optionsDefaultApiVersion = new ApiVersion(1, 0); // Define default API version as 1.0
         options.ReportApiVersions = true;     }); 
         }  

         [ApiVersion("1.0")][Route("api/v{version:apiVersion}/products")] 
         public class ProductsV1Controller : ControllerBase {     
         [HttpGet]     
         public IActionResult Get() => O("Products V1"); }  
         [ApiVersion("2.0")] [Route("api/v{version:apiVersion}/products")] 
         public class ProductsV2Controller : ControllerBase {    
         [HttpGet]     
         public IActionResult Get() => Ok("Products V2"); 
         } 
```  

Dans cet exemple, deux versions de l'API produits sont définies. La première version (`ProductsV1Controller`) répondra aux requêtes envoyées à `/api/v1products`, tandis que la seconde version (`ProductsV2Controller`) répondra aux requêtes à `/api/v2/products`. 
Cela permet aux utilisateurs de choisirexplicitement la version de l'API à utiliser via l'URL.  
Une telle stratégie peut être très utile pour les applications où les changements de version onttendance à être fréquents et significatifs, nécessitant une distinction claire entre les versions pour une gestion simplifiée et une meilleurecompréhension utilisateur. 
Cependant, cela nécessite une planification minutieuse pour éviter la duplication inutile de code et maintenir une base de codepropre et maintenable.