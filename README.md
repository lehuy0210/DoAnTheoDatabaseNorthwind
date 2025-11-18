# 🚀 Dự Án Quản Lý Northwind - .NET 10

## 📋 Tổng quan

Dự án học tập và thực hành **Lập trình Cơ sở Dữ liệu** sử dụng **.NET 10** và **C# 13** với cơ sở dữ liệu mẫu **Northwind**.

**Công ty Northwind Traders** là một công ty thương mại xuất nhập khẩu thực phẩm. Hệ thống quản lý sẽ bao gồm:
- Quản lý sản phẩm
- Quản lý đơn hàng
- Quản lý khách hàng
- Quản lý nhân viên
- Báo cáo và thống kê

---

## 🎯 Mục tiêu dự án

### Phần 1: Cơ bản (Tuần 1-3) - WinForms Desktop
- ✅ Hiểu và áp dụng ADO.NET
- ✅ Xây dựng kiến trúc 3-layer (DAL, BLL, GUI)
- ✅ CRUD đầy đủ cho tất cả modules
- ✅ Tìm kiếm, filter, validation
- ✅ Thống kê, xuất Excel, in báo cáo

### Phần 2: Nâng cao (Tuần 4-7) - Web Application
- ✅ Entity Framework Core 10 (Database First)
- ✅ LINQ to Entities + Stored Procedures
- ✅ Repository Pattern + Unit of Work
- ✅ ASP.NET Core Web API (RESTful)
- ✅ JWT Authentication
- ✅ ASP.NET Core MVC Web App
- ✅ SignalR Real-time updates
- ✅ Deploy lên Azure/Docker

---

## 🛠️ Công nghệ Stack

### Backend
- **.NET 10** (latest)
- **C# 13** với Primary Constructors, Collection Expressions
- **Entity Framework Core 10**
- **ASP.NET Core 10**
- **SQL Server 2022**

### Frontend
- **WinForms** (.NET 10)
- **ASP.NET Core MVC**
- **Bootstrap 5**
- **Chart.js**
- **SignalR Client**

### Tools & Libraries
- **Visual Studio 2022** (17.12+)
- **SSMS** (SQL Server Management Studio)
- **Postman** (API Testing)
- **Docker** (Containerization)
- **Azure** (Deployment)

---

## 📁 Cấu trúc dự án

```
DoAnTheoDatabaseNorthwind/
│
├── 📄 KE_HOACH_PHAT_TRIEN.md          # Kế hoạch 7 tuần chi tiết
├── 📄 DOTNET10_FEATURES.md            # Tính năng .NET 10 & C# 13
├── 📄 README.md                       # File này
├── 📄 ThietKeCSDLNorthwind_SQLServer.pdf  # ERD Database
│
├── 📁 Week1-3_WinForms/               # Tuần 1-3: WinForms cơ bản
│   ├── NorthwindDAL/                  # Data Access Layer
│   ├── NorthwindBLL/                  # Business Logic Layer
│   └── NorthwindGUI/                  # WinForms GUI
│
├── 📁 Week4-5_EntityFramework/        # Tuần 4-5: EF Core + LINQ
│   ├── Northwind.Models/              # Entity models
│   ├── Northwind.DAL/                 # Repositories
│   ├── Northwind.BLL/                 # Services
│   └── Northwind.WinForms/            # GUI với EF
│
├── 📁 Week6_WebAPI/                   # Tuần 6: Web API
│   ├── NorthwindAPI/                  # ASP.NET Core Web API
│   ├── NorthwindAPI.Tests/            # Unit tests
│   └── Dockerfile
│
└── 📁 Week7_MVC/                      # Tuần 7: MVC Web App
    ├── NorthwindMVC/                  # ASP.NET Core MVC
    ├── wwwroot/                       # Static files
    └── Dockerfile
```

---

## 🚀 Quick Start

### 1. Yêu cầu hệ thống

```bash
# Kiểm tra .NET 10
dotnet --version
# Output: 10.0.x

# Kiểm tra SQL Server
sqlcmd -S localhost -Q "SELECT @@VERSION"
```

**Cài đặt:**
- [.NET 10 SDK](https://dotnet.microsoft.com/download/dotnet/10.0)
- [Visual Studio 2022](https://visualstudio.microsoft.com/) (17.12+)
- [SQL Server 2022](https://www.microsoft.com/sql-server/sql-server-downloads) (Express hoặc Developer)

### 2. Cài đặt Database

```sql
-- Download Northwind database
-- https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/northwind-pubs

-- Restore database
RESTORE DATABASE Northwind
FROM DISK = 'C:\Path\To\Northwind.bak'
```

### 3. Clone repository

```bash
git clone https://github.com/lehuy0210/DoAnTheoDatabaseNorthwind.git
cd DoAnTheoDatabaseNorthwind
```

### 4. Chạy WinForms App (Tuần 1-3)

```bash
cd Week1-3_WinForms/NorthwindGUI
dotnet run
```

### 5. Chạy Web API (Tuần 6)

```bash
cd Week6_WebAPI/NorthwindAPI

# Update connection string trong appsettings.json
dotnet ef database update
dotnet run

# API sẽ chạy tại: https://localhost:7001
# Swagger: https://localhost:7001/swagger
```

### 6. Chạy MVC Web App (Tuần 7)

```bash
cd Week7_MVC/NorthwindMVC
dotnet run

# Web app: https://localhost:7002
```

---

## 📚 Tài liệu học tập

### Kế hoạch chi tiết
➡️ Xem file: **[KE_HOACH_PHAT_TRIEN.md](KE_HOACH_PHAT_TRIEN.md)**

### Tính năng .NET 10
➡️ Xem file: **[DOTNET10_FEATURES.md](DOTNET10_FEATURES.md)**

### Database Design
➡️ Xem file: **[ThietKeCSDLNorthwind_SQLServer.pdf](ThietKeCSDLNorthwind_SQLServer.pdf)**

---

## 🎓 Lộ trình học tập

### Sinh viên năm 2-3
- **Focus:** Tuần 1-3 (WinForms + ADO.NET)
- **Mục tiêu:** Hiểu rõ kiến trúc 3-layer, CRUD cơ bản
- **Thời gian:** 3-4 tuần

### Sinh viên năm 4 / Thực tập
- **Focus:** Tuần 1-7 (Full-stack)
- **Mục tiêu:** Desktop + Web + API
- **Thời gian:** 7-8 tuần

### Junior Developer
- **Focus:** Tuần 1-7 + Deployment
- **Mục tiêu:** Production-ready application
- **Thời gian:** 8-10 tuần

---

## 📊 Features Checklist

### Tuần 1: Quản lý Sản phẩm
- [ ] Database connection với ADO.NET
- [ ] ProductDAL với CRUD operations
- [ ] ProductBLL với validation
- [ ] WinForm GUI với DataGridView
- [ ] ComboBox cho Categories và Suppliers

### Tuần 2: Quản lý Đơn hàng
- [ ] OrderDAL và OrderDetailsDAL
- [ ] Master-Detail form cho Orders
- [ ] Tính toán tự động tổng tiền
- [ ] Tìm kiếm đơn hàng theo nhiều tiêu chí
- [ ] ComboBox động

### Tuần 3: Thống kê & Báo cáo
- [ ] StatisticsDAL với queries phức tạp
- [ ] Dashboard với Chart controls
- [ ] Xuất Excel (EPPlus/ClosedXML)
- [ ] In danh sách và hóa đơn
- [ ] Form Settings

### Tuần 4: Entity Framework Core
- [ ] Scaffold database với EF Core 10
- [ ] Tạo DTOs cho tất cả entities
- [ ] Repository Pattern implementation
- [ ] Refactor WinForms để sử dụng EF
- [ ] Testing performance

### Tuần 5: LINQ & Advanced Patterns
- [ ] LINQ queries cho tất cả operations
- [ ] Stored Procedures integration
- [ ] Generic Repository + Unit of Work
- [ ] LINQ to XML (import/export)
- [ ] Performance optimization

### Tuần 6: Web API
- [ ] RESTful API design
- [ ] JWT Authentication
- [ ] Swagger/OpenAPI documentation
- [ ] Filtering, Sorting, Pagination
- [ ] Deploy lên Azure

### Tuần 7: MVC Web Application
- [ ] ASP.NET Core MVC với Bootstrap 5
- [ ] Tích hợp với Web API
- [ ] SignalR real-time updates
- [ ] Dashboard với Chart.js
- [ ] Full deployment

---

## 🔧 Development

### Build

```bash
# Build tất cả projects
dotnet build

# Build specific project
dotnet build Week6_WebAPI/NorthwindAPI/NorthwindAPI.csproj
```

### Test

```bash
# Run all tests
dotnet test

# Run với coverage
dotnet test --collect:"XPlat Code Coverage"
```

### Deploy

```bash
# Publish Web API
dotnet publish Week6_WebAPI/NorthwindAPI -c Release -o ./publish

# Docker build
docker build -t northwind-api:latest -f Week6_WebAPI/Dockerfile .
docker run -p 8080:80 northwind-api:latest

# Azure deployment
az webapp up --name northwind-api --resource-group myRG --runtime "DOTNET|10.0"
```

---

## 🤝 Contributing

Dự án này được tạo cho mục đích học tập. Contributions, issues và feature requests đều được welcome!

### Quy trình contribute:
1. Fork repo
2. Tạo feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Tạo Pull Request

---

## 📝 License

Dự án này được sử dụng cho mục đích giáo dục tại Đại học Mở TP.HCM.
Database Northwind © Microsoft Corporation.

---

## 👨‍💻 Tác giả

**Sinh viên:** [Tên của bạn]
**MSSV:** [MSSV của bạn]
**Lớp:** [Lớp của bạn]
**Trường:** Đại học Mở TP.HCM
**Môn học:** ITEC3406 - Lập trình Cơ sở Dữ liệu

---

## 📞 Support

- 📧 Email: your.email@example.com
- 🌐 GitHub: https://github.com/lehuy0210/DoAnTheoDatabaseNorthwind
- 📚 Tài liệu: Xem file KE_HOACH_PHAT_TRIEN.md

---

## 🙏 Acknowledgments

- [Microsoft Northwind Database](https://github.com/Microsoft/sql-server-samples)
- [.NET Documentation](https://learn.microsoft.com/dotnet/)
- [Entity Framework Core](https://learn.microsoft.com/ef/core/)
- Đề cương môn ITEC3406 - Đại học Mở TP.HCM

---

**Version:** 2.0.0
**Last Updated:** 2025-01-18
**Technology:** .NET 10 & C# 13

⭐ **Star this repo if you find it helpful!**
