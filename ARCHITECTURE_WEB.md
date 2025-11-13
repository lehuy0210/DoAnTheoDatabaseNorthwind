# KIẾN TRÚC ỨNG DỤNG WEB - HỆ THỐNG NORTHWIND

## 1. TỔNG QUAN

Ứng dụng Web quản lý hệ thống Northwind sử dụng kiến trúc **Clean Architecture** với **ASP.NET Core Web API** (Backend) và **React/Vue.js** (Frontend).

### Mục tiêu
- RESTful API chuẩn
- Responsive web UI
- Real-time updates
- Scalable & maintainable
- Authentication & Authorization
- API Documentation (Swagger)

---

## 2. KIẾN TRÚC TỔNG QUAN

```
┌─────────────────────────────────────────────────┐
│              PRESENTATION LAYER                 │
│  ┌──────────────┐        ┌──────────────┐      │
│  │   React/Vue  │        │   Swagger    │      │
│  │   Frontend   │        │     UI       │      │
│  └──────────────┘        └──────────────┘      │
└────────────┬───────────────────┬────────────────┘
             │                   │
        HTTP │ REST API          │ HTTP
             │                   │
┌────────────▼───────────────────▼────────────────┐
│           ASP.NET CORE WEB API                  │
│  ┌──────────────────────────────────────┐      │
│  │         API CONTROLLERS              │      │
│  │  ProductsController                  │      │
│  │  OrdersController                    │      │
│  │  CustomersController                 │      │
│  └──────────────┬───────────────────────┘      │
│                 │                                │
│  ┌──────────────▼───────────────────────┐      │
│  │      APPLICATION SERVICES            │      │
│  │  ProductService                      │      │
│  │  OrderService                        │      │
│  │  CustomerService                     │      │
│  └──────────────┬───────────────────────┘      │
│                 │                                │
│  ┌──────────────▼───────────────────────┐      │
│  │         DOMAIN LAYER                 │      │
│  │  Entities, Interfaces, DTOs          │      │
│  └──────────────┬───────────────────────┘      │
│                 │                                │
│  ┌──────────────▼───────────────────────┐      │
│  │    INFRASTRUCTURE LAYER              │      │
│  │  EF Core DbContext                   │      │
│  │  Repositories                        │      │
│  │  Unit of Work                        │      │
│  └──────────────┬───────────────────────┘      │
└─────────────────┼──────────────────────────────┘
                  │
                  ▼
         ┌────────────────┐
         │  SQL SERVER    │
         │   Northwind    │
         └────────────────┘
```

---

## 3. CẤU TRÚC SOLUTION

```
Solution: NorthwindWeb
│
├── Northwind.Domain               (Core Domain Layer)
│   ├── Entities/
│   │   ├── Product.cs
│   │   ├── Category.cs
│   │   ├── Supplier.cs
│   │   ├── Customer.cs
│   │   ├── Order.cs
│   │   ├── OrderDetail.cs
│   │   ├── Employee.cs
│   │   └── Shipper.cs
│   ├── Interfaces/
│   │   ├── IRepository.cs
│   │   ├── IUnitOfWork.cs
│   │   ├── IProductRepository.cs
│   │   ├── IOrderRepository.cs
│   │   └── ICustomerRepository.cs
│   └── DTOs/
│       ├── ProductDTO.cs
│       ├── OrderDTO.cs
│       ├── CustomerDTO.cs
│       └── ResponseModels/
│           ├── ApiResponse.cs
│           ├── PagedResponse.cs
│           └── ErrorResponse.cs
│
├── Northwind.Application          (Application Services)
│   ├── Services/
│   │   ├── ProductService.cs
│   │   ├── CategoryService.cs
│   │   ├── OrderService.cs
│   │   ├── CustomerService.cs
│   │   └── EmployeeService.cs
│   ├── Mappings/
│   │   └── AutoMapperProfile.cs
│   ├── Validators/
│   │   ├── ProductValidator.cs
│   │   ├── OrderValidator.cs
│   │   └── CustomerValidator.cs
│   └── Exceptions/
│       ├── NotFoundException.cs
│       ├── ValidationException.cs
│       └── BusinessException.cs
│
├── Northwind.Infrastructure       (Data Access)
│   ├── Data/
│   │   ├── NorthwindDbContext.cs
│   │   └── DbInitializer.cs
│   ├── Repositories/
│   │   ├── Repository.cs
│   │   ├── UnitOfWork.cs
│   │   ├── ProductRepository.cs
│   │   ├── OrderRepository.cs
│   │   └── CustomerRepository.cs
│   └── Configurations/
│       ├── ProductConfiguration.cs
│       ├── OrderConfiguration.cs
│       └── CustomerConfiguration.cs
│
├── Northwind.API                  (Web API)
│   ├── Controllers/
│   │   ├── ProductsController.cs
│   │   ├── CategoriesController.cs
│   │   ├── SuppliersController.cs
│   │   ├── CustomersController.cs
│   │   ├── OrdersController.cs
│   │   ├── EmployeesController.cs
│   │   └── ReportsController.cs
│   ├── Middleware/
│   │   ├── ExceptionMiddleware.cs
│   │   └── LoggingMiddleware.cs
│   ├── Filters/
│   │   └── ValidateModelAttribute.cs
│   ├── Extensions/
│   │   └── ServiceExtensions.cs
│   ├── Program.cs
│   └── appsettings.json
│
└── Northwind.Web                  (Frontend - React/Vue)
    ├── src/
    │   ├── components/
    │   │   ├── Products/
    │   │   │   ├── ProductList.jsx
    │   │   │   ├── ProductDetail.jsx
    │   │   │   └── ProductForm.jsx
    │   │   ├── Orders/
    │   │   │   ├── OrderList.jsx
    │   │   │   ├── OrderDetail.jsx
    │   │   │   └── OrderCreate.jsx
    │   │   ├── Customers/
    │   │   │   ├── CustomerList.jsx
    │   │   │   └── CustomerDetail.jsx
    │   │   └── Shared/
    │   │       ├── Header.jsx
    │   │       ├── Sidebar.jsx
    │   │       └── DataTable.jsx
    │   ├── services/
    │   │   ├── api.js
    │   │   ├── productService.js
    │   │   ├── orderService.js
    │   │   └── customerService.js
    │   ├── store/              (Redux/Vuex)
    │   │   ├── products/
    │   │   ├── orders/
    │   │   └── customers/
    │   ├── utils/
    │   │   ├── formatters.js
    │   │   └── validators.js
    │   ├── App.jsx
    │   └── main.jsx
    └── package.json
```

---

## 4. CHI TIẾT BACKEND (ASP.NET CORE WEB API)

### 4.1. Domain Layer - Entities

#### Product.cs
```csharp
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;

namespace Northwind.Domain.Entities
{
    [Table("Products")]
    public class Product
    {
        [Key]
        public int ProductID { get; set; }

        [Required]
        [MaxLength(40)]
        public string ProductName { get; set; }

        public int? SupplierID { get; set; }
        public int? CategoryID { get; set; }

        [MaxLength(20)]
        public string QuantityPerUnit { get; set; }

        [Column(TypeName = "money")]
        public decimal? UnitPrice { get; set; }

        public short? UnitsInStock { get; set; }
        public short? UnitsOnOrder { get; set; }
        public short? ReorderLevel { get; set; }
        public bool Discontinued { get; set; }

        // Navigation Properties
        public virtual Category Category { get; set; }
        public virtual Supplier Supplier { get; set; }
        public virtual ICollection<OrderDetail> OrderDetails { get; set; }
    }
}
```

#### Order.cs
```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;

namespace Northwind.Domain.Entities
{
    [Table("Orders")]
    public class Order
    {
        [Key]
        public int OrderID { get; set; }

        [MaxLength(5)]
        public string CustomerID { get; set; }

        public int? EmployeeID { get; set; }
        public DateTime? OrderDate { get; set; }
        public DateTime? RequiredDate { get; set; }
        public DateTime? ShippedDate { get; set; }
        public int? ShipVia { get; set; }

        [Column(TypeName = "money")]
        public decimal? Freight { get; set; }

        [MaxLength(40)]
        public string ShipName { get; set; }

        [MaxLength(60)]
        public string ShipAddress { get; set; }

        [MaxLength(15)]
        public string ShipCity { get; set; }

        [MaxLength(15)]
        public string ShipRegion { get; set; }

        [MaxLength(10)]
        public string ShipPostalCode { get; set; }

        [MaxLength(15)]
        public string ShipCountry { get; set; }

        // Navigation Properties
        public virtual Customer Customer { get; set; }
        public virtual Employee Employee { get; set; }
        public virtual Shipper Shipper { get; set; }
        public virtual ICollection<OrderDetail> OrderDetails { get; set; }
    }
}
```

#### OrderDetail.cs
```csharp
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;

namespace Northwind.Domain.Entities
{
    [Table("Order Details")]
    public class OrderDetail
    {
        [Key, Column(Order = 0)]
        public int OrderID { get; set; }

        [Key, Column(Order = 1)]
        public int ProductID { get; set; }

        [Column(TypeName = "money")]
        public decimal UnitPrice { get; set; }

        public short Quantity { get; set; }

        [Column(TypeName = "real")]
        public float Discount { get; set; }

        // Navigation Properties
        public virtual Order Order { get; set; }
        public virtual Product Product { get; set; }
    }
}
```

---

### 4.2. Domain Layer - DTOs

#### ProductDTO.cs
```csharp
namespace Northwind.Domain.DTOs
{
    public class ProductDTO
    {
        public int ProductID { get; set; }
        public string ProductName { get; set; }
        public int? SupplierID { get; set; }
        public int? CategoryID { get; set; }
        public string QuantityPerUnit { get; set; }
        public decimal? UnitPrice { get; set; }
        public short? UnitsInStock { get; set; }
        public short? UnitsOnOrder { get; set; }
        public short? ReorderLevel { get; set; }
        public bool Discontinued { get; set; }

        // Additional Info
        public string CategoryName { get; set; }
        public string SupplierName { get; set; }
    }

    public class CreateProductDTO
    {
        [Required(ErrorMessage = "Tên sản phẩm không được để trống")]
        [MaxLength(40, ErrorMessage = "Tên sản phẩm không quá 40 ký tự")]
        public string ProductName { get; set; }

        public int? SupplierID { get; set; }
        public int? CategoryID { get; set; }
        public string QuantityPerUnit { get; set; }

        [Range(0, double.MaxValue, ErrorMessage = "Giá phải lớn hơn 0")]
        public decimal? UnitPrice { get; set; }

        [Range(0, short.MaxValue)]
        public short? UnitsInStock { get; set; }

        public short? UnitsOnOrder { get; set; }
        public short? ReorderLevel { get; set; }
        public bool Discontinued { get; set; }
    }

    public class UpdateProductDTO : CreateProductDTO
    {
        public int ProductID { get; set; }
    }
}
```

#### OrderDTO.cs
```csharp
using System;
using System.Collections.Generic;

namespace Northwind.Domain.DTOs
{
    public class OrderDTO
    {
        public int OrderID { get; set; }
        public string CustomerID { get; set; }
        public string CustomerName { get; set; }
        public int? EmployeeID { get; set; }
        public string EmployeeName { get; set; }
        public DateTime? OrderDate { get; set; }
        public DateTime? RequiredDate { get; set; }
        public DateTime? ShippedDate { get; set; }
        public int? ShipVia { get; set; }
        public string ShipperName { get; set; }
        public decimal? Freight { get; set; }
        public string ShipName { get; set; }
        public string ShipAddress { get; set; }
        public string ShipCity { get; set; }
        public string ShipCountry { get; set; }

        public List<OrderDetailDTO> OrderDetails { get; set; }
        public decimal TotalAmount { get; set; }
    }

    public class CreateOrderDTO
    {
        [Required]
        public string CustomerID { get; set; }

        public int? EmployeeID { get; set; }
        public DateTime? OrderDate { get; set; }
        public DateTime? RequiredDate { get; set; }
        public int? ShipVia { get; set; }
        public decimal? Freight { get; set; }
        public string ShipName { get; set; }
        public string ShipAddress { get; set; }
        public string ShipCity { get; set; }
        public string ShipCountry { get; set; }

        [Required]
        [MinLength(1, ErrorMessage = "Đơn hàng phải có ít nhất 1 sản phẩm")]
        public List<CreateOrderDetailDTO> OrderDetails { get; set; }
    }

    public class OrderDetailDTO
    {
        public int ProductID { get; set; }
        public string ProductName { get; set; }
        public decimal UnitPrice { get; set; }
        public short Quantity { get; set; }
        public float Discount { get; set; }
        public decimal TotalPrice { get; set; }
    }

    public class CreateOrderDetailDTO
    {
        [Required]
        public int ProductID { get; set; }

        [Required]
        [Range(0.01, double.MaxValue)]
        public decimal UnitPrice { get; set; }

        [Required]
        [Range(1, short.MaxValue)]
        public short Quantity { get; set; }

        [Range(0, 1)]
        public float Discount { get; set; }
    }
}
```

#### ApiResponse.cs
```csharp
namespace Northwind.Domain.DTOs.ResponseModels
{
    public class ApiResponse<T>
    {
        public bool Success { get; set; }
        public string Message { get; set; }
        public T Data { get; set; }
        public List<string> Errors { get; set; }

        public ApiResponse()
        {
            Errors = new List<string>();
        }

        public static ApiResponse<T> SuccessResult(T data, string message = "Success")
        {
            return new ApiResponse<T>
            {
                Success = true,
                Message = message,
                Data = data
            };
        }

        public static ApiResponse<T> ErrorResult(string message, List<string> errors = null)
        {
            return new ApiResponse<T>
            {
                Success = false,
                Message = message,
                Errors = errors ?? new List<string>()
            };
        }
    }

    public class PagedResponse<T>
    {
        public int PageNumber { get; set; }
        public int PageSize { get; set; }
        public int TotalPages { get; set; }
        public int TotalRecords { get; set; }
        public List<T> Data { get; set; }

        public PagedResponse(List<T> data, int count, int pageNumber, int pageSize)
        {
            Data = data;
            PageNumber = pageNumber;
            PageSize = pageSize;
            TotalRecords = count;
            TotalPages = (int)Math.Ceiling(count / (double)pageSize);
        }
    }
}
```

---

### 4.3. Infrastructure Layer - DbContext

#### NorthwindDbContext.cs
```csharp
using Microsoft.EntityFrameworkCore;
using Northwind.Domain.Entities;

namespace Northwind.Infrastructure.Data
{
    public class NorthwindDbContext : DbContext
    {
        public NorthwindDbContext(DbContextOptions<NorthwindDbContext> options)
            : base(options)
        {
        }

        public DbSet<Product> Products { get; set; }
        public DbSet<Category> Categories { get; set; }
        public DbSet<Supplier> Suppliers { get; set; }
        public DbSet<Customer> Customers { get; set; }
        public DbSet<Order> Orders { get; set; }
        public DbSet<OrderDetail> OrderDetails { get; set; }
        public DbSet<Employee> Employees { get; set; }
        public DbSet<Shipper> Shippers { get; set; }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            base.OnModelCreating(modelBuilder);

            // Apply configurations
            modelBuilder.ApplyConfigurationsFromAssembly(typeof(NorthwindDbContext).Assembly);

            // Composite key for OrderDetail
            modelBuilder.Entity<OrderDetail>()
                .HasKey(od => new { od.OrderID, od.ProductID });

            // Relationships
            modelBuilder.Entity<Product>()
                .HasOne(p => p.Category)
                .WithMany(c => c.Products)
                .HasForeignKey(p => p.CategoryID)
                .OnDelete(DeleteBehavior.SetNull);

            modelBuilder.Entity<Product>()
                .HasOne(p => p.Supplier)
                .WithMany(s => s.Products)
                .HasForeignKey(p => p.SupplierID)
                .OnDelete(DeleteBehavior.SetNull);

            modelBuilder.Entity<Order>()
                .HasOne(o => o.Customer)
                .WithMany(c => c.Orders)
                .HasForeignKey(o => o.CustomerID)
                .OnDelete(DeleteBehavior.Cascade);

            modelBuilder.Entity<OrderDetail>()
                .HasOne(od => od.Order)
                .WithMany(o => o.OrderDetails)
                .HasForeignKey(od => od.OrderID)
                .OnDelete(DeleteBehavior.Cascade);

            modelBuilder.Entity<OrderDetail>()
                .HasOne(od => od.Product)
                .WithMany(p => p.OrderDetails)
                .HasForeignKey(od => od.ProductID)
                .OnDelete(DeleteBehavior.Restrict);
        }
    }
}
```

---

### 4.4. Infrastructure Layer - Repository Pattern

#### IRepository.cs
```csharp
using System;
using System.Collections.Generic;
using System.Linq.Expressions;
using System.Threading.Tasks;

namespace Northwind.Domain.Interfaces
{
    public interface IRepository<T> where T : class
    {
        Task<T> GetByIdAsync(int id);
        Task<IEnumerable<T>> GetAllAsync();
        Task<IEnumerable<T>> FindAsync(Expression<Func<T, bool>> predicate);
        Task<T> FirstOrDefaultAsync(Expression<Func<T, bool>> predicate);
        Task AddAsync(T entity);
        void Update(T entity);
        void Remove(T entity);
        Task<int> CountAsync();
        Task<int> CountAsync(Expression<Func<T, bool>> predicate);
    }
}
```

#### Repository.cs
```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Linq.Expressions;
using System.Threading.Tasks;
using Microsoft.EntityFrameworkCore;
using Northwind.Domain.Interfaces;
using Northwind.Infrastructure.Data;

namespace Northwind.Infrastructure.Repositories
{
    public class Repository<T> : IRepository<T> where T : class
    {
        protected readonly NorthwindDbContext _context;
        protected readonly DbSet<T> _dbSet;

        public Repository(NorthwindDbContext context)
        {
            _context = context;
            _dbSet = context.Set<T>();
        }

        public virtual async Task<T> GetByIdAsync(int id)
        {
            return await _dbSet.FindAsync(id);
        }

        public virtual async Task<IEnumerable<T>> GetAllAsync()
        {
            return await _dbSet.ToListAsync();
        }

        public virtual async Task<IEnumerable<T>> FindAsync(Expression<Func<T, bool>> predicate)
        {
            return await _dbSet.Where(predicate).ToListAsync();
        }

        public virtual async Task<T> FirstOrDefaultAsync(Expression<Func<T, bool>> predicate)
        {
            return await _dbSet.FirstOrDefaultAsync(predicate);
        }

        public virtual async Task AddAsync(T entity)
        {
            await _dbSet.AddAsync(entity);
        }

        public virtual void Update(T entity)
        {
            _dbSet.Update(entity);
        }

        public virtual void Remove(T entity)
        {
            _dbSet.Remove(entity);
        }

        public virtual async Task<int> CountAsync()
        {
            return await _dbSet.CountAsync();
        }

        public virtual async Task<int> CountAsync(Expression<Func<T, bool>> predicate)
        {
            return await _dbSet.CountAsync(predicate);
        }
    }
}
```

#### ProductRepository.cs
```csharp
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.EntityFrameworkCore;
using Northwind.Domain.Entities;
using Northwind.Domain.Interfaces;
using Northwind.Infrastructure.Data;

namespace Northwind.Infrastructure.Repositories
{
    public interface IProductRepository : IRepository<Product>
    {
        Task<IEnumerable<Product>> GetProductsWithDetailsAsync();
        Task<Product> GetProductWithDetailsAsync(int id);
        Task<IEnumerable<Product>> SearchProductsAsync(string keyword);
        Task<IEnumerable<Product>> GetProductsByCategoryAsync(int categoryId);
    }

    public class ProductRepository : Repository<Product>, IProductRepository
    {
        public ProductRepository(NorthwindDbContext context) : base(context)
        {
        }

        public async Task<IEnumerable<Product>> GetProductsWithDetailsAsync()
        {
            return await _dbSet
                .Include(p => p.Category)
                .Include(p => p.Supplier)
                .OrderBy(p => p.ProductName)
                .ToListAsync();
        }

        public async Task<Product> GetProductWithDetailsAsync(int id)
        {
            return await _dbSet
                .Include(p => p.Category)
                .Include(p => p.Supplier)
                .FirstOrDefaultAsync(p => p.ProductID == id);
        }

        public async Task<IEnumerable<Product>> SearchProductsAsync(string keyword)
        {
            return await _dbSet
                .Include(p => p.Category)
                .Include(p => p.Supplier)
                .Where(p => p.ProductName.Contains(keyword))
                .OrderBy(p => p.ProductName)
                .ToListAsync();
        }

        public async Task<IEnumerable<Product>> GetProductsByCategoryAsync(int categoryId)
        {
            return await _dbSet
                .Include(p => p.Category)
                .Include(p => p.Supplier)
                .Where(p => p.CategoryID == categoryId)
                .OrderBy(p => p.ProductName)
                .ToListAsync();
        }
    }
}
```

#### UnitOfWork.cs
```csharp
using System;
using System.Threading.Tasks;
using Northwind.Domain.Interfaces;
using Northwind.Infrastructure.Data;

namespace Northwind.Infrastructure.Repositories
{
    public interface IUnitOfWork : IDisposable
    {
        IProductRepository Products { get; }
        IOrderRepository Orders { get; }
        ICustomerRepository Customers { get; }
        ICategoryRepository Categories { get; }
        ISupplierRepository Suppliers { get; }
        IEmployeeRepository Employees { get; }

        Task<int> CompleteAsync();
    }

    public class UnitOfWork : IUnitOfWork
    {
        private readonly NorthwindDbContext _context;

        public IProductRepository Products { get; }
        public IOrderRepository Orders { get; }
        public ICustomerRepository Customers { get; }
        public ICategoryRepository Categories { get; }
        public ISupplierRepository Suppliers { get; }
        public IEmployeeRepository Employees { get; }

        public UnitOfWork(
            NorthwindDbContext context,
            IProductRepository productRepository,
            IOrderRepository orderRepository,
            ICustomerRepository customerRepository,
            ICategoryRepository categoryRepository,
            ISupplierRepository supplierRepository,
            IEmployeeRepository employeeRepository)
        {
            _context = context;
            Products = productRepository;
            Orders = orderRepository;
            Customers = customerRepository;
            Categories = categoryRepository;
            Suppliers = supplierRepository;
            Employees = employeeRepository;
        }

        public async Task<int> CompleteAsync()
        {
            return await _context.SaveChangesAsync();
        }

        public void Dispose()
        {
            _context.Dispose();
        }
    }
}
```

---

### 4.5. Application Layer - Services

#### ProductService.cs
```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using AutoMapper;
using Northwind.Application.Exceptions;
using Northwind.Domain.DTOs;
using Northwind.Domain.Entities;
using Northwind.Infrastructure.Repositories;

namespace Northwind.Application.Services
{
    public interface IProductService
    {
        Task<IEnumerable<ProductDTO>> GetAllProductsAsync();
        Task<ProductDTO> GetProductByIdAsync(int id);
        Task<ProductDTO> CreateProductAsync(CreateProductDTO dto);
        Task<ProductDTO> UpdateProductAsync(int id, UpdateProductDTO dto);
        Task<bool> DeleteProductAsync(int id);
        Task<IEnumerable<ProductDTO>> SearchProductsAsync(string keyword);
        Task<PagedResponse<ProductDTO>> GetProductsPagedAsync(int pageNumber, int pageSize);
    }

    public class ProductService : IProductService
    {
        private readonly IUnitOfWork _unitOfWork;
        private readonly IMapper _mapper;

        public ProductService(IUnitOfWork unitOfWork, IMapper mapper)
        {
            _unitOfWork = unitOfWork;
            _mapper = mapper;
        }

        public async Task<IEnumerable<ProductDTO>> GetAllProductsAsync()
        {
            var products = await _unitOfWork.Products.GetProductsWithDetailsAsync();
            return _mapper.Map<IEnumerable<ProductDTO>>(products);
        }

        public async Task<ProductDTO> GetProductByIdAsync(int id)
        {
            var product = await _unitOfWork.Products.GetProductWithDetailsAsync(id);

            if (product == null)
                throw new NotFoundException($"Không tìm thấy sản phẩm với ID {id}");

            return _mapper.Map<ProductDTO>(product);
        }

        public async Task<ProductDTO> CreateProductAsync(CreateProductDTO dto)
        {
            var product = _mapper.Map<Product>(dto);

            await _unitOfWork.Products.AddAsync(product);
            await _unitOfWork.CompleteAsync();

            return _mapper.Map<ProductDTO>(product);
        }

        public async Task<ProductDTO> UpdateProductAsync(int id, UpdateProductDTO dto)
        {
            var product = await _unitOfWork.Products.GetByIdAsync(id);

            if (product == null)
                throw new NotFoundException($"Không tìm thấy sản phẩm với ID {id}");

            _mapper.Map(dto, product);
            _unitOfWork.Products.Update(product);
            await _unitOfWork.CompleteAsync();

            return _mapper.Map<ProductDTO>(product);
        }

        public async Task<bool> DeleteProductAsync(int id)
        {
            var product = await _unitOfWork.Products.GetByIdAsync(id);

            if (product == null)
                throw new NotFoundException($"Không tìm thấy sản phẩm với ID {id}");

            _unitOfWork.Products.Remove(product);
            await _unitOfWork.CompleteAsync();

            return true;
        }

        public async Task<IEnumerable<ProductDTO>> SearchProductsAsync(string keyword)
        {
            var products = await _unitOfWork.Products.SearchProductsAsync(keyword);
            return _mapper.Map<IEnumerable<ProductDTO>>(products);
        }

        public async Task<PagedResponse<ProductDTO>> GetProductsPagedAsync(
            int pageNumber, int pageSize)
        {
            var totalCount = await _unitOfWork.Products.CountAsync();

            var products = await _unitOfWork.Products.GetProductsWithDetailsAsync();
            var pagedProducts = products
                .Skip((pageNumber - 1) * pageSize)
                .Take(pageSize)
                .ToList();

            var productDTOs = _mapper.Map<List<ProductDTO>>(pagedProducts);

            return new PagedResponse<ProductDTO>(productDTOs, totalCount, pageNumber, pageSize);
        }
    }
}
```

#### OrderService.cs
```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using AutoMapper;
using Northwind.Application.Exceptions;
using Northwind.Domain.DTOs;
using Northwind.Domain.Entities;
using Northwind.Infrastructure.Repositories;

namespace Northwind.Application.Services
{
    public interface IOrderService
    {
        Task<IEnumerable<OrderDTO>> GetAllOrdersAsync();
        Task<OrderDTO> GetOrderByIdAsync(int id);
        Task<OrderDTO> CreateOrderAsync(CreateOrderDTO dto);
        Task<bool> UpdateOrderStatusAsync(int id, string status);
        Task<bool> DeleteOrderAsync(int id);
    }

    public class OrderService : IOrderService
    {
        private readonly IUnitOfWork _unitOfWork;
        private readonly IMapper _mapper;

        public OrderService(IUnitOfWork unitOfWork, IMapper mapper)
        {
            _unitOfWork = unitOfWork;
            _mapper = mapper;
        }

        public async Task<IEnumerable<OrderDTO>> GetAllOrdersAsync()
        {
            var orders = await _unitOfWork.Orders.GetOrdersWithDetailsAsync();
            return _mapper.Map<IEnumerable<OrderDTO>>(orders);
        }

        public async Task<OrderDTO> GetOrderByIdAsync(int id)
        {
            var order = await _unitOfWork.Orders.GetOrderWithDetailsAsync(id);

            if (order == null)
                throw new NotFoundException($"Không tìm thấy đơn hàng với ID {id}");

            var orderDTO = _mapper.Map<OrderDTO>(order);

            // Calculate total
            orderDTO.TotalAmount = order.OrderDetails.Sum(od =>
                od.Quantity * od.UnitPrice * (decimal)(1 - od.Discount));

            return orderDTO;
        }

        public async Task<OrderDTO> CreateOrderAsync(CreateOrderDTO dto)
        {
            // Validate customer exists
            var customer = await _unitOfWork.Customers.GetByIdAsync(dto.CustomerID);
            if (customer == null)
                throw new ValidationException("Khách hàng không tồn tại");

            // Validate products exist and in stock
            foreach (var detail in dto.OrderDetails)
            {
                var product = await _unitOfWork.Products.GetByIdAsync(detail.ProductID);
                if (product == null)
                    throw new ValidationException($"Sản phẩm {detail.ProductID} không tồn tại");

                if (product.UnitsInStock < detail.Quantity)
                    throw new ValidationException(
                        $"Sản phẩm {product.ProductName} không đủ số lượng trong kho");
            }

            // Create order
            var order = _mapper.Map<Order>(dto);
            await _unitOfWork.Orders.AddAsync(order);
            await _unitOfWork.CompleteAsync();

            // Update product stock
            foreach (var detail in order.OrderDetails)
            {
                var product = await _unitOfWork.Products.GetByIdAsync(detail.ProductID);
                product.UnitsInStock -= detail.Quantity;
                _unitOfWork.Products.Update(product);
            }

            await _unitOfWork.CompleteAsync();

            return _mapper.Map<OrderDTO>(order);
        }

        public async Task<bool> UpdateOrderStatusAsync(int id, string status)
        {
            var order = await _unitOfWork.Orders.GetByIdAsync(id);

            if (order == null)
                throw new NotFoundException($"Không tìm thấy đơn hàng với ID {id}");

            // Update status based on input
            if (status.ToLower() == "shipped")
                order.ShippedDate = DateTime.Now;

            _unitOfWork.Orders.Update(order);
            await _unitOfWork.CompleteAsync();

            return true;
        }

        public async Task<bool> DeleteOrderAsync(int id)
        {
            var order = await _unitOfWork.Orders.GetOrderWithDetailsAsync(id);

            if (order == null)
                throw new NotFoundException($"Không tìm thấy đơn hàng với ID {id}");

            // Restore product stock
            foreach (var detail in order.OrderDetails)
            {
                var product = await _unitOfWork.Products.GetByIdAsync(detail.ProductID);
                product.UnitsInStock += detail.Quantity;
                _unitOfWork.Products.Update(product);
            }

            _unitOfWork.Orders.Remove(order);
            await _unitOfWork.CompleteAsync();

            return true;
        }
    }
}
```

---

### 4.6. API Layer - Controllers

#### ProductsController.cs
```csharp
using System;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Northwind.Application.Services;
using Northwind.Domain.DTOs;
using Northwind.Domain.DTOs.ResponseModels;

namespace Northwind.API.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class ProductsController : ControllerBase
    {
        private readonly IProductService _productService;

        public ProductsController(IProductService productService)
        {
            _productService = productService;
        }

        /// <summary>
        /// Lấy tất cả sản phẩm
        /// </summary>
        [HttpGet]
        public async Task<IActionResult> GetAll()
        {
            var products = await _productService.GetAllProductsAsync();
            return Ok(ApiResponse<IEnumerable<ProductDTO>>.SuccessResult(products));
        }

        /// <summary>
        /// Lấy sản phẩm theo ID
        /// </summary>
        [HttpGet("{id}")]
        public async Task<IActionResult> GetById(int id)
        {
            var product = await _productService.GetProductByIdAsync(id);
            return Ok(ApiResponse<ProductDTO>.SuccessResult(product));
        }

        /// <summary>
        /// Tạo sản phẩm mới
        /// </summary>
        [HttpPost]
        public async Task<IActionResult> Create([FromBody] CreateProductDTO dto)
        {
            if (!ModelState.IsValid)
                return BadRequest(ApiResponse<object>.ErrorResult("Dữ liệu không hợp lệ"));

            var product = await _productService.CreateProductAsync(dto);
            return CreatedAtAction(nameof(GetById), new { id = product.ProductID },
                ApiResponse<ProductDTO>.SuccessResult(product, "Tạo sản phẩm thành công"));
        }

        /// <summary>
        /// Cập nhật sản phẩm
        /// </summary>
        [HttpPut("{id}")]
        public async Task<IActionResult> Update(int id, [FromBody] UpdateProductDTO dto)
        {
            if (!ModelState.IsValid)
                return BadRequest(ApiResponse<object>.ErrorResult("Dữ liệu không hợp lệ"));

            var product = await _productService.UpdateProductAsync(id, dto);
            return Ok(ApiResponse<ProductDTO>.SuccessResult(product, "Cập nhật thành công"));
        }

        /// <summary>
        /// Xóa sản phẩm
        /// </summary>
        [HttpDelete("{id}")]
        public async Task<IActionResult> Delete(int id)
        {
            await _productService.DeleteProductAsync(id);
            return Ok(ApiResponse<object>.SuccessResult(null, "Xóa thành công"));
        }

        /// <summary>
        /// Tìm kiếm sản phẩm
        /// </summary>
        [HttpGet("search")]
        public async Task<IActionResult> Search([FromQuery] string keyword)
        {
            var products = await _productService.SearchProductsAsync(keyword ?? "");
            return Ok(ApiResponse<IEnumerable<ProductDTO>>.SuccessResult(products));
        }

        /// <summary>
        /// Lấy sản phẩm theo trang
        /// </summary>
        [HttpGet("paged")]
        public async Task<IActionResult> GetPaged([FromQuery] int pageNumber = 1,
            [FromQuery] int pageSize = 10)
        {
            var pagedProducts = await _productService.GetProductsPagedAsync(pageNumber, pageSize);
            return Ok(pagedProducts);
        }
    }
}
```

#### OrdersController.cs
```csharp
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Northwind.Application.Services;
using Northwind.Domain.DTOs;
using Northwind.Domain.DTOs.ResponseModels;

namespace Northwind.API.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class OrdersController : ControllerBase
    {
        private readonly IOrderService _orderService;

        public OrdersController(IOrderService orderService)
        {
            _orderService = orderService;
        }

        [HttpGet]
        public async Task<IActionResult> GetAll()
        {
            var orders = await _orderService.GetAllOrdersAsync();
            return Ok(ApiResponse<IEnumerable<OrderDTO>>.SuccessResult(orders));
        }

        [HttpGet("{id}")]
        public async Task<IActionResult> GetById(int id)
        {
            var order = await _orderService.GetOrderByIdAsync(id);
            return Ok(ApiResponse<OrderDTO>.SuccessResult(order));
        }

        [HttpPost]
        public async Task<IActionResult> Create([FromBody] CreateOrderDTO dto)
        {
            if (!ModelState.IsValid)
                return BadRequest(ApiResponse<object>.ErrorResult("Dữ liệu không hợp lệ"));

            var order = await _orderService.CreateOrderAsync(dto);
            return CreatedAtAction(nameof(GetById), new { id = order.OrderID },
                ApiResponse<OrderDTO>.SuccessResult(order, "Tạo đơn hàng thành công"));
        }

        [HttpPatch("{id}/status")]
        public async Task<IActionResult> UpdateStatus(int id, [FromBody] string status)
        {
            await _orderService.UpdateOrderStatusAsync(id, status);
            return Ok(ApiResponse<object>.SuccessResult(null, "Cập nhật trạng thái thành công"));
        }

        [HttpDelete("{id}")]
        public async Task<IActionResult> Delete(int id)
        {
            await _orderService.DeleteOrderAsync(id);
            return Ok(ApiResponse<object>.SuccessResult(null, "Xóa đơn hàng thành công"));
        }
    }
}
```

---

### 4.7. Middleware - Exception Handling

#### ExceptionMiddleware.cs
```csharp
using System;
using System.Net;
using System.Text.Json;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Http;
using Northwind.Application.Exceptions;
using Northwind.Domain.DTOs.ResponseModels;

namespace Northwind.API.Middleware
{
    public class ExceptionMiddleware
    {
        private readonly RequestDelegate _next;

        public ExceptionMiddleware(RequestDelegate next)
        {
            _next = next;
        }

        public async Task InvokeAsync(HttpContext context)
        {
            try
            {
                await _next(context);
            }
            catch (Exception ex)
            {
                await HandleExceptionAsync(context, ex);
            }
        }

        private static Task HandleExceptionAsync(HttpContext context, Exception exception)
        {
            var code = HttpStatusCode.InternalServerError;
            var result = string.Empty;

            switch (exception)
            {
                case NotFoundException _:
                    code = HttpStatusCode.NotFound;
                    result = JsonSerializer.Serialize(
                        ApiResponse<object>.ErrorResult(exception.Message));
                    break;
                case ValidationException validationException:
                    code = HttpStatusCode.BadRequest;
                    result = JsonSerializer.Serialize(
                        ApiResponse<object>.ErrorResult(
                            exception.Message,
                            validationException.Errors));
                    break;
                case BusinessException _:
                    code = HttpStatusCode.BadRequest;
                    result = JsonSerializer.Serialize(
                        ApiResponse<object>.ErrorResult(exception.Message));
                    break;
                default:
                    result = JsonSerializer.Serialize(
                        ApiResponse<object>.ErrorResult("Đã xảy ra lỗi server"));
                    break;
            }

            context.Response.ContentType = "application/json";
            context.Response.StatusCode = (int)code;

            return context.Response.WriteAsync(result);
        }
    }
}
```

---

### 4.8. Program.cs - Dependency Injection

```csharp
using Microsoft.EntityFrameworkCore;
using Northwind.Infrastructure.Data;
using Northwind.Infrastructure.Repositories;
using Northwind.Application.Services;
using Northwind.API.Middleware;

var builder = WebApplication.CreateBuilder(args);

// Add DbContext
builder.Services.AddDbContext<NorthwindDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("NorthwindDB")));

// Add Repositories
builder.Services.AddScoped<IProductRepository, ProductRepository>();
builder.Services.AddScoped<IOrderRepository, OrderRepository>();
builder.Services.AddScoped<ICustomerRepository, CustomerRepository>();
builder.Services.AddScoped<ICategoryRepository, CategoryRepository>();
builder.Services.AddScoped<ISupplierRepository, SupplierRepository>();
builder.Services.AddScoped<IEmployeeRepository, EmployeeRepository>();

// Add Unit of Work
builder.Services.AddScoped<IUnitOfWork, UnitOfWork>();

// Add Services
builder.Services.AddScoped<IProductService, ProductService>();
builder.Services.AddScoped<IOrderService, OrderService>();
builder.Services.AddScoped<ICustomerService, CustomerService>();

// Add AutoMapper
builder.Services.AddAutoMapper(AppDomain.CurrentDomain.GetAssemblies());

// Add Controllers
builder.Services.AddControllers();

// Add Swagger
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen(c =>
{
    c.SwaggerDoc("v1", new() { Title = "Northwind API", Version = "v1" });
});

// Add CORS
builder.Services.AddCors(options =>
{
    options.AddPolicy("AllowAll",
        builder => builder
            .AllowAnyOrigin()
            .AllowAnyMethod()
            .AllowAnyHeader());
});

var app = builder.Build();

// Configure middleware
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

app.UseMiddleware<ExceptionMiddleware>();

app.UseHttpsRedirection();
app.UseCors("AllowAll");
app.UseAuthorization();
app.MapControllers();

app.Run();
```

---

## 5. FRONTEND (React Example)

### 5.1. API Service

#### api.js
```javascript
import axios from 'axios';

const API_BASE_URL = 'https://localhost:7001/api';

const api = axios.create({
  baseURL: API_BASE_URL,
  headers: {
    'Content-Type': 'application/json',
  },
});

// Response interceptor
api.interceptors.response.use(
  (response) => response.data,
  (error) => {
    const message = error.response?.data?.message || 'Đã xảy ra lỗi';
    return Promise.reject(new Error(message));
  }
);

export default api;
```

#### productService.js
```javascript
import api from './api';

export const productService = {
  getAll: async () => {
    return await api.get('/products');
  },

  getById: async (id) => {
    return await api.get(`/products/${id}`);
  },

  create: async (product) => {
    return await api.post('/products', product);
  },

  update: async (id, product) => {
    return await api.put(`/products/${id}`, product);
  },

  delete: async (id) => {
    return await api.delete(`/products/${id}`);
  },

  search: async (keyword) => {
    return await api.get('/products/search', { params: { keyword } });
  },

  getPaged: async (pageNumber, pageSize) => {
    return await api.get('/products/paged', {
      params: { pageNumber, pageSize },
    });
  },
};
```

### 5.2. Product Components

#### ProductList.jsx
```jsx
import React, { useState, useEffect } from 'react';
import { productService } from '../../services/productService';
import ProductTable from './ProductTable';
import ProductForm from './ProductForm';

const ProductList = () => {
  const [products, setProducts] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  const [showForm, setShowForm] = useState(false);
  const [selectedProduct, setSelectedProduct] = useState(null);

  useEffect(() => {
    loadProducts();
  }, []);

  const loadProducts = async () => {
    try {
      setLoading(true);
      const response = await productService.getAll();
      setProducts(response.data);
    } catch (err) {
      setError(err.message);
    } finally {
      setLoading(false);
    }
  };

  const handleAdd = () => {
    setSelectedProduct(null);
    setShowForm(true);
  };

  const handleEdit = (product) => {
    setSelectedProduct(product);
    setShowForm(true);
  };

  const handleDelete = async (id) => {
    if (window.confirm('Bạn có chắc muốn xóa sản phẩm này?')) {
      try {
        await productService.delete(id);
        loadProducts();
      } catch (err) {
        alert(err.message);
      }
    }
  };

  const handleSave = async () => {
    setShowForm(false);
    loadProducts();
  };

  if (loading) return <div>Đang tải...</div>;
  if (error) return <div>Lỗi: {error}</div>;

  return (
    <div className="product-list">
      <h2>Danh sách sản phẩm</h2>
      <button onClick={handleAdd}>Thêm sản phẩm</button>

      <ProductTable
        products={products}
        onEdit={handleEdit}
        onDelete={handleDelete}
      />

      {showForm && (
        <ProductForm
          product={selectedProduct}
          onSave={handleSave}
          onCancel={() => setShowForm(false)}
        />
      )}
    </div>
  );
};

export default ProductList;
```

---

## 6. API ENDPOINTS

### Products
```
GET    /api/products              - Lấy tất cả sản phẩm
GET    /api/products/{id}         - Lấy sản phẩm theo ID
POST   /api/products              - Tạo sản phẩm mới
PUT    /api/products/{id}         - Cập nhật sản phẩm
DELETE /api/products/{id}         - Xóa sản phẩm
GET    /api/products/search       - Tìm kiếm sản phẩm
GET    /api/products/paged        - Phân trang sản phẩm
```

### Orders
```
GET    /api/orders                - Lấy tất cả đơn hàng
GET    /api/orders/{id}           - Lấy đơn hàng theo ID
POST   /api/orders                - Tạo đơn hàng mới
PATCH  /api/orders/{id}/status    - Cập nhật trạng thái
DELETE /api/orders/{id}           - Xóa đơn hàng
```

### Customers
```
GET    /api/customers             - Lấy tất cả khách hàng
GET    /api/customers/{id}        - Lấy khách hàng theo ID
POST   /api/customers             - Tạo khách hàng mới
PUT    /api/customers/{id}        - Cập nhật khách hàng
DELETE /api/customers/{id}        - Xóa khách hàng
```

---

## 7. BEST PRACTICES

### 7.1. Async/Await
```csharp
public async Task<IActionResult> GetAll()
{
    var products = await _productService.GetAllProductsAsync();
    return Ok(products);
}
```

### 7.2. Dependency Injection
```csharp
public class ProductService : IProductService
{
    private readonly IUnitOfWork _unitOfWork;

    public ProductService(IUnitOfWork unitOfWork)
    {
        _unitOfWork = unitOfWork;
    }
}
```

### 7.3. AutoMapper
```csharp
public class AutoMapperProfile : Profile
{
    public AutoMapperProfile()
    {
        CreateMap<Product, ProductDTO>()
            .ForMember(dest => dest.CategoryName,
                opt => opt.MapFrom(src => src.Category.CategoryName))
            .ForMember(dest => dest.SupplierName,
                opt => opt.MapFrom(src => src.Supplier.CompanyName));

        CreateMap<CreateProductDTO, Product>();
        CreateMap<UpdateProductDTO, Product>();
    }
}
```

---

## 8. DEPLOYMENT

### 8.1. Backend (Azure/IIS)
```bash
# Build
dotnet publish -c Release

# Deploy to Azure
az webapp deploy --name northwind-api --resource-group northwind-rg
```

### 8.2. Frontend (Vercel/Netlify)
```bash
# Build
npm run build

# Deploy
vercel --prod
```

### 8.3. Database Migration
```bash
# Add migration
dotnet ef migrations add InitialCreate

# Update database
dotnet ef database update
```

---

## 9. SECURITY

1. **Authentication:** JWT Bearer Token
2. **Authorization:** Role-based access control
3. **HTTPS:** Enforce SSL/TLS
4. **CORS:** Configure allowed origins
5. **Rate Limiting:** Prevent API abuse
6. **Input Validation:** FluentValidation
7. **SQL Injection:** Entity Framework parameterized queries

---

## 10. TỔNG KẾT

### Ưu điểm:
- ✅ Scalable và maintainable
- ✅ Separation of concerns rõ ràng
- ✅ RESTful API chuẩn
- ✅ Async/await performance tốt
- ✅ Dễ testing với DI
- ✅ Modern technology stack

### Nhược điểm:
- ❌ Phức tạp hơn so với monolithic
- ❌ Cần kiến thức về nhiều công nghệ
- ❌ Setup ban đầu mất thời gian

### Khi nào dùng:
- ✅ Ứng dụng web quy mô lớn
- ✅ Cần API cho nhiều client (web, mobile, desktop)
- ✅ Team lớn, long-term project
- ✅ Cần scalability và performance cao
