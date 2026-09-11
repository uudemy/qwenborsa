# TradingApp - Borsa / Trading Uygulaması

Modern, güvenli ve ölçeklenebilir bir demo/paper trading uygulaması.

## 🏗️ Mimari Yapı

Bu proje **Clean Architecture** prensiplerine göre tasarlanmıştır:

```
TradingApp.sln
├── src/
│   ├── TradingApp.Api          # API katmanı (Controllers, Middleware, Configuration)
│   ├── TradingApp.Application  # Uygulama mantığı (CQRS, Validators, DTOs)
│   ├── TradingApp.Domain       # Domain varlıkları (Entities, Enums, Interfaces)
│   └── TradingApp.Infrastructure # Altyapı (EF Core, Redis, External Services)
├── tests/
│   └── TradingApp.UnitTests    # Birim testleri
├── frontend/                   # React + TypeScript frontend
└── docker-compose.yml          # Docker yapılandırması
```

### Proje Sorumlulukları

| Proje | Sorumluluk |
|-------|-----------|
| **TradingApp.Domain** | Entity'ler, enum'lar, domain interface'leri. EF Core'a bağımlı değil. |
| **TradingApp.Application** | CQRS komutları/queries, validator'lar, DTO'lar, mapping profilleri |
| **TradingApp.Infrastructure** | EF Core DbContext, repository implementasyonları, Redis, external servisler |
| **TradingApp.Api** | Controllers, middleware, DI configuration, Swagger, authentication |
| **TradingApp.UnitTests** | Domain ve Application katmanı için birim testleri |

## 🛠️ Teknoloji Stack

### Backend
- **.NET 9** & ASP.NET Core 9 Web API
- **Entity Framework Core 9** & PostgreSQL
- **Redis** (Cache & Pub/Sub)
- **SignalR** (Real-time fiyat güncellemeleri)
- **JWT Authentication** & Refresh Token
- **FluentValidation** (Validasyon)
- **Serilog** (Logging)
- **Swagger/OpenAPI** (API Dokümantasyonu)
- **MediatR** (CQRS Pattern)
- **Mapster** (Object Mapping)
- **Health Checks** (PostgreSQL & Redis)

### Frontend (Planlanan)
- React + TypeScript
- Vite
- Tailwind CSS
- Recharts / TradingView Lightweight Charts

## 🚀 Başlangıç

### Gereksinimler
- .NET 9 SDK
- Docker & Docker Compose (PostgreSQL ve Redis için)

### 1. Docker Servislerini Başlat

```bash
docker compose up -d postgres redis
```

### 2. Veritabanı Migration

```bash
cd src/TradingApp.Api
dotnet ef database update
```

### 3. API'yi Çalıştır

```bash
dotnet run
```

API varsayılan olarak `http://localhost:5000` adresinde çalışacaktır.

### 4. Swagger UI

Tarayıcınızda şu adresi açın:
```
http://localhost:5000/swagger
```

### 5. Health Check

```bash
curl http://localhost:5000/health/live
```

## 📦 NuGet Paketleri

### TradingApp.Domain
- MediatR.Contracts

### TradingApp.Application
- MediatR
- FluentValidation
- FluentValidation.DependencyInjectionExtensions
- Mapster
- Microsoft.Extensions.Logging.Abstractions

### TradingApp.Infrastructure
- Microsoft.EntityFrameworkCore
- Npgsql.EntityFrameworkCore.PostgreSQL
- Microsoft.Extensions.Configuration.Abstractions
- StackExchange.Redis

### TradingApp.Api
- Microsoft.AspNetCore.Authentication.JwtBearer
- Serilog.AspNetCore
- Swashbuckle.AspNetCore
- Microsoft.AspNetCore.SignalR
- AspNetCore.HealthChecks.NpgSql
- AspNetCore.HealthChecks.Redis
- AspNetCore.HealthChecks.UI.Client

## 🔐 Güvenlik Ayarları

JWT ayarları `appsettings.json` dosyasında tanımlanmıştır:

```json
{
  "JwtSettings": {
    "Secret": "YourSuperSecretKeyThatIsAtLeast32CharactersLong!",
    "Issuer": "TradingApp",
    "Audience": "TradingAppUsers",
    "ExpirationInMinutes": 60,
    "RefreshTokenExpirationInDays": 7
  }
}
```

⚠️ **Önemli**: Production ortamında bu değerleri environment variables ile yönetin!

## 📊 Veritabanı Bağlantısı

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Host=localhost;Port=5432;Database=trading_db;Username=trading_user;Password=trading_password"
  }
}
```

## 🔴 Redis Bağlantısı

```json
{
  "Redis": {
    "ConnectionString": "localhost:6379"
  }
}
```

## 📝 Loglama

Serilog ile loglama yapılandırılmıştır. Loglar:
- Konsola (Console)
- Dosyaya (`logs/trading-app-.log`)

yazılır.

## 🧪 Testler

```bash
dotnet test
```

## 📈 Sonraki Aşamalar

1. **Domain Entities** - User, Asset, Wallet, Order, Trade vb. entity'lerin oluşturulması
2. **Authentication** - JWT Register/Login/Refresh Token implementasyonu
3. **Market Data Engine** - Simüle edilmiş fiyat verileri
4. **Order Matching Engine** - Emir eşleştirme motoru
5. **Portfolio Management** - Kullanıcı portföy takibi
6. **SignalR Hub** - Real-time fiyat güncellemeleri
7. **Frontend** - React arayüzü

## ⚠️ Önemli Notlar

- Bu uygulama **demo/paper trading** amaçlıdır. Gerçek para veya borsa entegrasyonu içermez.
- Finansal hesaplamalarda `decimal` tipi kullanılır (`double` veya `float` kullanılmaz).
- Concurrency sorunlarına karşı transaction ve row locking mekanizmaları kullanılacaktır.

## 📄 Lisans

MIT License
