* Eklemek gereken paketler.
 ```razor 
  
dotnet add package StackExchange.Redis                                 //Redis’e bağlanmak ve temel işlemler için.   
dotnet add package Microsoft.Extensions.Caching.StackExchangeRedis    //Redis’i .NET Core cache sistemiyle entegre kullanmak
dotnet add package Microsoft.AspNetCore.Session                      //ASP.NET Core içinde Redis Session yönetimi
```

* Redis Cache 

```razor
builder.Services.AddStackExchangeRedisCache(options =>
{
    options.Configuration = configuration.GetConnectionString("RedisConnection");
    options.InstanceName = "YourAppCache:";
});
```


* REDİS KURULUM CONTAİNER OLUŞTURULMASI VEDE NETWORKE EKLENMESİ

```razor
builder.Services.AddStackExchangeRedisCache(options =>
{
docker run --name redis -p 1453:6379 -d redis
});
```
```razor
docker ps

docker exec -it e53 redis-cli

PING --> PONG çıktısı almamız gerek

```
* network e ekledik ve network içine baktık
```razor
docker network connect microservice-network redis

docker network inspect microservice-network

```
