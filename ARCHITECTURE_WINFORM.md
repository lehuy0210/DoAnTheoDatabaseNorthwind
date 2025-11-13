# KIẾN TRÚC ỨNG DỤNG WINFORM - HỆ THỐNG NORTHWIND

## 1. TỔNG QUAN

Ứng dụng WinForm quản lý hệ thống Northwind sử dụng kiến trúc **3 lớp (3-Tier Architecture)** với **ADO.NET** để tương tác với SQL Server database.

### Mục tiêu
- Quản lý sản phẩm, đơn hàng, khách hàng, nhà cung cấp
- Giao diện desktop thân thiện, dễ sử dụng
- Hiệu suất cao, xử lý offline
- Bảo mật và validation đầy đủ

---

## 2. CẤU TRÚC SOLUTION

```
Solution: NorthwindWinForm
│
├── Northwind.DTO           (Data Transfer Objects)
│   ├── ProductDTO.cs
│   ├── CategoryDTO.cs
│   ├── SupplierDTO.cs
│   ├── CustomerDTO.cs
│   ├── OrderDTO.cs
│   ├── OrderDetailDTO.cs
│   ├── EmployeeDTO.cs
│   └── ShipperDTO.cs
│
├── Northwind.DAL           (Data Access Layer)
│   ├── DatabaseHelper.cs
│   ├── ProductDAL.cs
│   ├── CategoryDAL.cs
│   ├── SupplierDAL.cs
│   ├── CustomerDAL.cs
│   ├── OrderDAL.cs
│   ├── OrderDetailDAL.cs
│   ├── EmployeeDAL.cs
│   └── ShipperDAL.cs
│
├── Northwind.BLL           (Business Logic Layer)
│   ├── ProductBLL.cs
│   ├── CategoryBLL.cs
│   ├── SupplierBLL.cs
│   ├── CustomerBLL.cs
│   ├── OrderBLL.cs
│   ├── OrderDetailBLL.cs
│   ├── EmployeeBLL.cs
│   └── ShipperBLL.cs
│
└── Northwind.GUI           (Presentation Layer - WinForms)
    ├── frmMain.cs              (Main Dashboard)
    ├── Products/
    │   ├── frmProductList.cs
    │   ├── frmProductDetail.cs
    │   └── frmProductSearch.cs
    ├── Categories/
    │   ├── frmCategoryList.cs
    │   └── frmCategoryDetail.cs
    ├── Suppliers/
    │   ├── frmSupplierList.cs
    │   └── frmSupplierDetail.cs
    ├── Customers/
    │   ├── frmCustomerList.cs
    │   └── frmCustomerDetail.cs
    ├── Orders/
    │   ├── frmOrderList.cs
    │   ├── frmOrderDetail.cs
    │   └── frmOrderCreate.cs
    ├── Employees/
    │   ├── frmEmployeeList.cs
    │   └── frmEmployeeDetail.cs
    ├── Reports/
    │   ├── frmSalesReport.cs
    │   ├── frmInventoryReport.cs
    │   └── frmCustomerReport.cs
    └── App.config
```

---

## 3. CHI TIẾT TỪNG LỚP

### 3.1. DTO Layer (Data Transfer Objects)

**Mục đích:** Chứa các đối tượng dữ liệu thuần túy, không chứa logic nghiệp vụ.

#### ProductDTO.cs
```csharp
namespace Northwind.DTO
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

        // Navigation properties (for display)
        public string CategoryName { get; set; }
        public string SupplierName { get; set; }
    }
}
```

#### CategoryDTO.cs
```csharp
namespace Northwind.DTO
{
    public class CategoryDTO
    {
        public int CategoryID { get; set; }
        public string CategoryName { get; set; }
        public string Description { get; set; }
        public byte[] Picture { get; set; }
    }
}
```

#### OrderDTO.cs
```csharp
namespace Northwind.DTO
{
    public class OrderDTO
    {
        public int OrderID { get; set; }
        public string CustomerID { get; set; }
        public int? EmployeeID { get; set; }
        public DateTime? OrderDate { get; set; }
        public DateTime? RequiredDate { get; set; }
        public DateTime? ShippedDate { get; set; }
        public int? ShipVia { get; set; }
        public decimal? Freight { get; set; }
        public string ShipName { get; set; }
        public string ShipAddress { get; set; }
        public string ShipCity { get; set; }
        public string ShipRegion { get; set; }
        public string ShipPostalCode { get; set; }
        public string ShipCountry { get; set; }

        // Navigation properties
        public string CustomerName { get; set; }
        public string EmployeeName { get; set; }
        public string ShipperName { get; set; }

        // Calculated properties
        public decimal TotalAmount { get; set; }
    }
}
```

#### OrderDetailDTO.cs
```csharp
namespace Northwind.DTO
{
    public class OrderDetailDTO
    {
        public int OrderID { get; set; }
        public int ProductID { get; set; }
        public decimal UnitPrice { get; set; }
        public short Quantity { get; set; }
        public float Discount { get; set; }

        // Navigation properties
        public string ProductName { get; set; }

        // Calculated properties
        public decimal TotalPrice
        {
            get { return (decimal)Quantity * UnitPrice * (1 - (decimal)Discount); }
        }
    }
}
```

#### CustomerDTO.cs
```csharp
namespace Northwind.DTO
{
    public class CustomerDTO
    {
        public string CustomerID { get; set; }
        public string CompanyName { get; set; }
        public string ContactName { get; set; }
        public string ContactTitle { get; set; }
        public string Address { get; set; }
        public string City { get; set; }
        public string Region { get; set; }
        public string PostalCode { get; set; }
        public string Country { get; set; }
        public string Phone { get; set; }
        public string Fax { get; set; }
    }
}
```

---

### 3.2. DAL Layer (Data Access Layer)

**Mục đích:** Xử lý tất cả tương tác với database sử dụng ADO.NET.

#### DatabaseHelper.cs
```csharp
using System;
using System.Data.SqlClient;
using System.Configuration;

namespace Northwind.DAL
{
    public class DatabaseHelper
    {
        private static string connectionString =
            ConfigurationManager.ConnectionStrings["NorthwindDB"].ConnectionString;

        public static SqlConnection GetConnection()
        {
            return new SqlConnection(connectionString);
        }

        public static bool TestConnection()
        {
            try
            {
                using (SqlConnection conn = GetConnection())
                {
                    conn.Open();
                    return true;
                }
            }
            catch
            {
                return false;
            }
        }
    }
}
```

#### ProductDAL.cs
```csharp
using System;
using System.Collections.Generic;
using System.Data;
using System.Data.SqlClient;
using Northwind.DTO;

namespace Northwind.DAL
{
    public class ProductDAL
    {
        // Lấy tất cả sản phẩm
        public List<ProductDTO> LayDanhSach()
        {
            List<ProductDTO> list = new List<ProductDTO>();

            string query = @"
                SELECT p.*, c.CategoryName, s.CompanyName as SupplierName
                FROM Products p
                LEFT JOIN Categories c ON p.CategoryID = c.CategoryID
                LEFT JOIN Suppliers s ON p.SupplierID = s.SupplierID
                ORDER BY p.ProductName";

            using (SqlConnection conn = DatabaseHelper.GetConnection())
            {
                using (SqlCommand cmd = new SqlCommand(query, conn))
                {
                    conn.Open();
                    using (SqlDataReader reader = cmd.ExecuteReader())
                    {
                        while (reader.Read())
                        {
                            list.Add(new ProductDTO
                            {
                                ProductID = (int)reader["ProductID"],
                                ProductName = reader["ProductName"].ToString(),
                                SupplierID = reader["SupplierID"] as int?,
                                CategoryID = reader["CategoryID"] as int?,
                                QuantityPerUnit = reader["QuantityPerUnit"].ToString(),
                                UnitPrice = reader["UnitPrice"] as decimal?,
                                UnitsInStock = reader["UnitsInStock"] as short?,
                                UnitsOnOrder = reader["UnitsOnOrder"] as short?,
                                ReorderLevel = reader["ReorderLevel"] as short?,
                                Discontinued = (bool)reader["Discontinued"],
                                CategoryName = reader["CategoryName"].ToString(),
                                SupplierName = reader["SupplierName"].ToString()
                            });
                        }
                    }
                }
            }

            return list;
        }

        // Tìm sản phẩm theo ID
        public ProductDTO TimTheoMa(int productID)
        {
            ProductDTO product = null;

            string query = @"
                SELECT p.*, c.CategoryName, s.CompanyName as SupplierName
                FROM Products p
                LEFT JOIN Categories c ON p.CategoryID = c.CategoryID
                LEFT JOIN Suppliers s ON p.SupplierID = s.SupplierID
                WHERE p.ProductID = @ProductID";

            using (SqlConnection conn = DatabaseHelper.GetConnection())
            {
                using (SqlCommand cmd = new SqlCommand(query, conn))
                {
                    cmd.Parameters.AddWithValue("@ProductID", productID);
                    conn.Open();

                    using (SqlDataReader reader = cmd.ExecuteReader())
                    {
                        if (reader.Read())
                        {
                            product = new ProductDTO
                            {
                                ProductID = (int)reader["ProductID"],
                                ProductName = reader["ProductName"].ToString(),
                                SupplierID = reader["SupplierID"] as int?,
                                CategoryID = reader["CategoryID"] as int?,
                                QuantityPerUnit = reader["QuantityPerUnit"].ToString(),
                                UnitPrice = reader["UnitPrice"] as decimal?,
                                UnitsInStock = reader["UnitsInStock"] as short?,
                                UnitsOnOrder = reader["UnitsOnOrder"] as short?,
                                ReorderLevel = reader["ReorderLevel"] as short?,
                                Discontinued = (bool)reader["Discontinued"],
                                CategoryName = reader["CategoryName"].ToString(),
                                SupplierName = reader["SupplierName"].ToString()
                            };
                        }
                    }
                }
            }

            return product;
        }

        // Thêm sản phẩm mới
        public bool Them(ProductDTO product)
        {
            string query = @"
                INSERT INTO Products (ProductName, SupplierID, CategoryID,
                    QuantityPerUnit, UnitPrice, UnitsInStock, UnitsOnOrder,
                    ReorderLevel, Discontinued)
                VALUES (@ProductName, @SupplierID, @CategoryID,
                    @QuantityPerUnit, @UnitPrice, @UnitsInStock, @UnitsOnOrder,
                    @ReorderLevel, @Discontinued)";

            using (SqlConnection conn = DatabaseHelper.GetConnection())
            {
                using (SqlCommand cmd = new SqlCommand(query, conn))
                {
                    cmd.Parameters.AddWithValue("@ProductName", product.ProductName);
                    cmd.Parameters.AddWithValue("@SupplierID",
                        (object)product.SupplierID ?? DBNull.Value);
                    cmd.Parameters.AddWithValue("@CategoryID",
                        (object)product.CategoryID ?? DBNull.Value);
                    cmd.Parameters.AddWithValue("@QuantityPerUnit",
                        product.QuantityPerUnit ?? (object)DBNull.Value);
                    cmd.Parameters.AddWithValue("@UnitPrice",
                        (object)product.UnitPrice ?? DBNull.Value);
                    cmd.Parameters.AddWithValue("@UnitsInStock",
                        (object)product.UnitsInStock ?? DBNull.Value);
                    cmd.Parameters.AddWithValue("@UnitsOnOrder",
                        (object)product.UnitsOnOrder ?? DBNull.Value);
                    cmd.Parameters.AddWithValue("@ReorderLevel",
                        (object)product.ReorderLevel ?? DBNull.Value);
                    cmd.Parameters.AddWithValue("@Discontinued", product.Discontinued);

                    conn.Open();
                    return cmd.ExecuteNonQuery() > 0;
                }
            }
        }

        // Cập nhật sản phẩm
        public bool CapNhat(ProductDTO product)
        {
            string query = @"
                UPDATE Products
                SET ProductName = @ProductName,
                    SupplierID = @SupplierID,
                    CategoryID = @CategoryID,
                    QuantityPerUnit = @QuantityPerUnit,
                    UnitPrice = @UnitPrice,
                    UnitsInStock = @UnitsInStock,
                    UnitsOnOrder = @UnitsOnOrder,
                    ReorderLevel = @ReorderLevel,
                    Discontinued = @Discontinued
                WHERE ProductID = @ProductID";

            using (SqlConnection conn = DatabaseHelper.GetConnection())
            {
                using (SqlCommand cmd = new SqlCommand(query, conn))
                {
                    cmd.Parameters.AddWithValue("@ProductID", product.ProductID);
                    cmd.Parameters.AddWithValue("@ProductName", product.ProductName);
                    cmd.Parameters.AddWithValue("@SupplierID",
                        (object)product.SupplierID ?? DBNull.Value);
                    cmd.Parameters.AddWithValue("@CategoryID",
                        (object)product.CategoryID ?? DBNull.Value);
                    cmd.Parameters.AddWithValue("@QuantityPerUnit",
                        product.QuantityPerUnit ?? (object)DBNull.Value);
                    cmd.Parameters.AddWithValue("@UnitPrice",
                        (object)product.UnitPrice ?? DBNull.Value);
                    cmd.Parameters.AddWithValue("@UnitsInStock",
                        (object)product.UnitsInStock ?? DBNull.Value);
                    cmd.Parameters.AddWithValue("@UnitsOnOrder",
                        (object)product.UnitsOnOrder ?? DBNull.Value);
                    cmd.Parameters.AddWithValue("@ReorderLevel",
                        (object)product.ReorderLevel ?? DBNull.Value);
                    cmd.Parameters.AddWithValue("@Discontinued", product.Discontinued);

                    conn.Open();
                    return cmd.ExecuteNonQuery() > 0;
                }
            }
        }

        // Xóa sản phẩm
        public bool Xoa(int productID)
        {
            string query = "DELETE FROM Products WHERE ProductID = @ProductID";

            using (SqlConnection conn = DatabaseHelper.GetConnection())
            {
                using (SqlCommand cmd = new SqlCommand(query, conn))
                {
                    cmd.Parameters.AddWithValue("@ProductID", productID);
                    conn.Open();
                    return cmd.ExecuteNonQuery() > 0;
                }
            }
        }

        // Tìm kiếm sản phẩm theo tên
        public List<ProductDTO> TimKiem(string keyword)
        {
            List<ProductDTO> list = new List<ProductDTO>();

            string query = @"
                SELECT p.*, c.CategoryName, s.CompanyName as SupplierName
                FROM Products p
                LEFT JOIN Categories c ON p.CategoryID = c.CategoryID
                LEFT JOIN Suppliers s ON p.SupplierID = s.SupplierID
                WHERE p.ProductName LIKE @Keyword
                ORDER BY p.ProductName";

            using (SqlConnection conn = DatabaseHelper.GetConnection())
            {
                using (SqlCommand cmd = new SqlCommand(query, conn))
                {
                    cmd.Parameters.AddWithValue("@Keyword", "%" + keyword + "%");
                    conn.Open();

                    using (SqlDataReader reader = cmd.ExecuteReader())
                    {
                        while (reader.Read())
                        {
                            list.Add(new ProductDTO
                            {
                                ProductID = (int)reader["ProductID"],
                                ProductName = reader["ProductName"].ToString(),
                                SupplierID = reader["SupplierID"] as int?,
                                CategoryID = reader["CategoryID"] as int?,
                                QuantityPerUnit = reader["QuantityPerUnit"].ToString(),
                                UnitPrice = reader["UnitPrice"] as decimal?,
                                UnitsInStock = reader["UnitsInStock"] as short?,
                                UnitsOnOrder = reader["UnitsOnOrder"] as short?,
                                ReorderLevel = reader["ReorderLevel"] as short?,
                                Discontinued = (bool)reader["Discontinued"],
                                CategoryName = reader["CategoryName"].ToString(),
                                SupplierName = reader["SupplierName"].ToString()
                            });
                        }
                    }
                }
            }

            return list;
        }

        // Lấy sản phẩm theo danh mục
        public List<ProductDTO> LayTheoCategory(int categoryID)
        {
            List<ProductDTO> list = new List<ProductDTO>();

            string query = @"
                SELECT p.*, c.CategoryName, s.CompanyName as SupplierName
                FROM Products p
                LEFT JOIN Categories c ON p.CategoryID = c.CategoryID
                LEFT JOIN Suppliers s ON p.SupplierID = s.SupplierID
                WHERE p.CategoryID = @CategoryID
                ORDER BY p.ProductName";

            using (SqlConnection conn = DatabaseHelper.GetConnection())
            {
                using (SqlCommand cmd = new SqlCommand(query, conn))
                {
                    cmd.Parameters.AddWithValue("@CategoryID", categoryID);
                    conn.Open();

                    using (SqlDataReader reader = cmd.ExecuteReader())
                    {
                        while (reader.Read())
                        {
                            list.Add(new ProductDTO
                            {
                                ProductID = (int)reader["ProductID"],
                                ProductName = reader["ProductName"].ToString(),
                                SupplierID = reader["SupplierID"] as int?,
                                CategoryID = reader["CategoryID"] as int?,
                                QuantityPerUnit = reader["QuantityPerUnit"].ToString(),
                                UnitPrice = reader["UnitPrice"] as decimal?,
                                UnitsInStock = reader["UnitsInStock"] as short?,
                                UnitsOnOrder = reader["UnitsOnOrder"] as short?,
                                ReorderLevel = reader["ReorderLevel"] as short?,
                                Discontinued = (bool)reader["Discontinued"],
                                CategoryName = reader["CategoryName"].ToString(),
                                SupplierName = reader["SupplierName"].ToString()
                            });
                        }
                    }
                }
            }

            return list;
        }
    }
}
```

#### OrderDAL.cs
```csharp
using System;
using System.Collections.Generic;
using System.Data;
using System.Data.SqlClient;
using Northwind.DTO;

namespace Northwind.DAL
{
    public class OrderDAL
    {
        // Lấy tất cả đơn hàng
        public List<OrderDTO> LayDanhSach()
        {
            List<OrderDTO> list = new List<OrderDTO>();

            string query = @"
                SELECT o.*, c.CompanyName as CustomerName,
                       e.LastName + ' ' + e.FirstName as EmployeeName,
                       s.CompanyName as ShipperName
                FROM Orders o
                LEFT JOIN Customers c ON o.CustomerID = c.CustomerID
                LEFT JOIN Employees e ON o.EmployeeID = e.EmployeeID
                LEFT JOIN Shippers s ON o.ShipVia = s.ShipperID
                ORDER BY o.OrderDate DESC";

            using (SqlConnection conn = DatabaseHelper.GetConnection())
            {
                using (SqlCommand cmd = new SqlCommand(query, conn))
                {
                    conn.Open();
                    using (SqlDataReader reader = cmd.ExecuteReader())
                    {
                        while (reader.Read())
                        {
                            list.Add(MapOrderFromReader(reader));
                        }
                    }
                }
            }

            return list;
        }

        // Tạo đơn hàng mới với chi tiết
        public int TaoDonHang(OrderDTO order, List<OrderDetailDTO> orderDetails)
        {
            using (SqlConnection conn = DatabaseHelper.GetConnection())
            {
                conn.Open();
                using (SqlTransaction transaction = conn.BeginTransaction())
                {
                    try
                    {
                        // 1. Insert Order
                        string insertOrderQuery = @"
                            INSERT INTO Orders (CustomerID, EmployeeID, OrderDate,
                                RequiredDate, ShippedDate, ShipVia, Freight,
                                ShipName, ShipAddress, ShipCity, ShipRegion,
                                ShipPostalCode, ShipCountry)
                            VALUES (@CustomerID, @EmployeeID, @OrderDate,
                                @RequiredDate, @ShippedDate, @ShipVia, @Freight,
                                @ShipName, @ShipAddress, @ShipCity, @ShipRegion,
                                @ShipPostalCode, @ShipCountry);
                            SELECT CAST(SCOPE_IDENTITY() AS INT)";

                        int orderID;
                        using (SqlCommand cmd = new SqlCommand(insertOrderQuery, conn, transaction))
                        {
                            cmd.Parameters.AddWithValue("@CustomerID", order.CustomerID);
                            cmd.Parameters.AddWithValue("@EmployeeID",
                                (object)order.EmployeeID ?? DBNull.Value);
                            cmd.Parameters.AddWithValue("@OrderDate",
                                (object)order.OrderDate ?? DBNull.Value);
                            cmd.Parameters.AddWithValue("@RequiredDate",
                                (object)order.RequiredDate ?? DBNull.Value);
                            cmd.Parameters.AddWithValue("@ShippedDate",
                                (object)order.ShippedDate ?? DBNull.Value);
                            cmd.Parameters.AddWithValue("@ShipVia",
                                (object)order.ShipVia ?? DBNull.Value);
                            cmd.Parameters.AddWithValue("@Freight",
                                (object)order.Freight ?? DBNull.Value);
                            cmd.Parameters.AddWithValue("@ShipName",
                                order.ShipName ?? (object)DBNull.Value);
                            cmd.Parameters.AddWithValue("@ShipAddress",
                                order.ShipAddress ?? (object)DBNull.Value);
                            cmd.Parameters.AddWithValue("@ShipCity",
                                order.ShipCity ?? (object)DBNull.Value);
                            cmd.Parameters.AddWithValue("@ShipRegion",
                                order.ShipRegion ?? (object)DBNull.Value);
                            cmd.Parameters.AddWithValue("@ShipPostalCode",
                                order.ShipPostalCode ?? (object)DBNull.Value);
                            cmd.Parameters.AddWithValue("@ShipCountry",
                                order.ShipCountry ?? (object)DBNull.Value);

                            orderID = (int)cmd.ExecuteScalar();
                        }

                        // 2. Insert Order Details
                        string insertDetailQuery = @"
                            INSERT INTO [Order Details] (OrderID, ProductID, UnitPrice,
                                Quantity, Discount)
                            VALUES (@OrderID, @ProductID, @UnitPrice, @Quantity, @Discount)";

                        foreach (var detail in orderDetails)
                        {
                            using (SqlCommand cmd = new SqlCommand(insertDetailQuery, conn, transaction))
                            {
                                cmd.Parameters.AddWithValue("@OrderID", orderID);
                                cmd.Parameters.AddWithValue("@ProductID", detail.ProductID);
                                cmd.Parameters.AddWithValue("@UnitPrice", detail.UnitPrice);
                                cmd.Parameters.AddWithValue("@Quantity", detail.Quantity);
                                cmd.Parameters.AddWithValue("@Discount", detail.Discount);
                                cmd.ExecuteNonQuery();
                            }
                        }

                        // 3. Update Product Stock
                        string updateStockQuery = @"
                            UPDATE Products
                            SET UnitsInStock = UnitsInStock - @Quantity
                            WHERE ProductID = @ProductID";

                        foreach (var detail in orderDetails)
                        {
                            using (SqlCommand cmd = new SqlCommand(updateStockQuery, conn, transaction))
                            {
                                cmd.Parameters.AddWithValue("@Quantity", detail.Quantity);
                                cmd.Parameters.AddWithValue("@ProductID", detail.ProductID);
                                cmd.ExecuteNonQuery();
                            }
                        }

                        transaction.Commit();
                        return orderID;
                    }
                    catch
                    {
                        transaction.Rollback();
                        throw;
                    }
                }
            }
        }

        // Helper method
        private OrderDTO MapOrderFromReader(SqlDataReader reader)
        {
            return new OrderDTO
            {
                OrderID = (int)reader["OrderID"],
                CustomerID = reader["CustomerID"].ToString(),
                EmployeeID = reader["EmployeeID"] as int?,
                OrderDate = reader["OrderDate"] as DateTime?,
                RequiredDate = reader["RequiredDate"] as DateTime?,
                ShippedDate = reader["ShippedDate"] as DateTime?,
                ShipVia = reader["ShipVia"] as int?,
                Freight = reader["Freight"] as decimal?,
                ShipName = reader["ShipName"].ToString(),
                ShipAddress = reader["ShipAddress"].ToString(),
                ShipCity = reader["ShipCity"].ToString(),
                ShipRegion = reader["ShipRegion"].ToString(),
                ShipPostalCode = reader["ShipPostalCode"].ToString(),
                ShipCountry = reader["ShipCountry"].ToString(),
                CustomerName = reader["CustomerName"].ToString(),
                EmployeeName = reader["EmployeeName"].ToString(),
                ShipperName = reader["ShipperName"].ToString()
            };
        }
    }
}
```

---

### 3.3. BLL Layer (Business Logic Layer)

**Mục đích:** Xử lý validation, business rules, gọi DAL.

#### ProductBLL.cs
```csharp
using System;
using System.Collections.Generic;
using Northwind.DTO;
using Northwind.DAL;

namespace Northwind.BLL
{
    public class ProductBLL
    {
        private ProductDAL productDAL = new ProductDAL();

        // Lấy danh sách
        public List<ProductDTO> LayDanhSach()
        {
            try
            {
                return productDAL.LayDanhSach();
            }
            catch (Exception ex)
            {
                throw new Exception("Lỗi khi lấy danh sách sản phẩm: " + ex.Message);
            }
        }

        // Tìm theo mã
        public ProductDTO TimTheoMa(int productID)
        {
            try
            {
                if (productID <= 0)
                    throw new Exception("Mã sản phẩm không hợp lệ");

                return productDAL.TimTheoMa(productID);
            }
            catch (Exception ex)
            {
                throw new Exception("Lỗi khi tìm sản phẩm: " + ex.Message);
            }
        }

        // Thêm mới
        public bool Them(ProductDTO product)
        {
            try
            {
                // Validation
                if (string.IsNullOrWhiteSpace(product.ProductName))
                    throw new Exception("Tên sản phẩm không được để trống");

                if (product.ProductName.Length > 40)
                    throw new Exception("Tên sản phẩm không được quá 40 ký tự");

                if (product.UnitPrice.HasValue && product.UnitPrice < 0)
                    throw new Exception("Giá sản phẩm không được âm");

                if (product.UnitsInStock.HasValue && product.UnitsInStock < 0)
                    throw new Exception("Số lượng tồn kho không được âm");

                // Gọi DAL
                return productDAL.Them(product);
            }
            catch (Exception ex)
            {
                throw new Exception("Lỗi khi thêm sản phẩm: " + ex.Message);
            }
        }

        // Cập nhật
        public bool CapNhat(ProductDTO product)
        {
            try
            {
                // Validation
                if (product.ProductID <= 0)
                    throw new Exception("Mã sản phẩm không hợp lệ");

                if (string.IsNullOrWhiteSpace(product.ProductName))
                    throw new Exception("Tên sản phẩm không được để trống");

                if (product.ProductName.Length > 40)
                    throw new Exception("Tên sản phẩm không được quá 40 ký tự");

                if (product.UnitPrice.HasValue && product.UnitPrice < 0)
                    throw new Exception("Giá sản phẩm không được âm");

                if (product.UnitsInStock.HasValue && product.UnitsInStock < 0)
                    throw new Exception("Số lượng tồn kho không được âm");

                // Gọi DAL
                return productDAL.CapNhat(product);
            }
            catch (Exception ex)
            {
                throw new Exception("Lỗi khi cập nhật sản phẩm: " + ex.Message);
            }
        }

        // Xóa
        public bool Xoa(int productID)
        {
            try
            {
                if (productID <= 0)
                    throw new Exception("Mã sản phẩm không hợp lệ");

                // Kiểm tra sản phẩm có tồn tại trong đơn hàng không
                // TODO: Implement check

                return productDAL.Xoa(productID);
            }
            catch (Exception ex)
            {
                throw new Exception("Lỗi khi xóa sản phẩm: " + ex.Message);
            }
        }

        // Tìm kiếm
        public List<ProductDTO> TimKiem(string keyword)
        {
            try
            {
                if (string.IsNullOrWhiteSpace(keyword))
                    return LayDanhSach();

                return productDAL.TimKiem(keyword);
            }
            catch (Exception ex)
            {
                throw new Exception("Lỗi khi tìm kiếm sản phẩm: " + ex.Message);
            }
        }
    }
}
```

#### OrderBLL.cs
```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Northwind.DTO;
using Northwind.DAL;

namespace Northwind.BLL
{
    public class OrderBLL
    {
        private OrderDAL orderDAL = new OrderDAL();

        // Tạo đơn hàng
        public int TaoDonHang(OrderDTO order, List<OrderDetailDTO> orderDetails)
        {
            try
            {
                // Validation
                if (string.IsNullOrWhiteSpace(order.CustomerID))
                    throw new Exception("Vui lòng chọn khách hàng");

                if (orderDetails == null || orderDetails.Count == 0)
                    throw new Exception("Đơn hàng phải có ít nhất 1 sản phẩm");

                // Validate order details
                foreach (var detail in orderDetails)
                {
                    if (detail.Quantity <= 0)
                        throw new Exception("Số lượng phải lớn hơn 0");

                    if (detail.UnitPrice < 0)
                        throw new Exception("Giá không được âm");

                    if (detail.Discount < 0 || detail.Discount > 1)
                        throw new Exception("Chiết khấu phải từ 0 đến 1");
                }

                // Tính tổng tiền
                order.TotalAmount = orderDetails.Sum(d => d.TotalPrice);

                // Gọi DAL
                return orderDAL.TaoDonHang(order, orderDetails);
            }
            catch (Exception ex)
            {
                throw new Exception("Lỗi khi tạo đơn hàng: " + ex.Message);
            }
        }

        // Lấy danh sách
        public List<OrderDTO> LayDanhSach()
        {
            try
            {
                return orderDAL.LayDanhSach();
            }
            catch (Exception ex)
            {
                throw new Exception("Lỗi khi lấy danh sách đơn hàng: " + ex.Message);
            }
        }
    }
}
```

---

### 3.4. GUI Layer (Presentation Layer)

**Mục đích:** Hiển thị giao diện, xử lý user input, gọi BLL.

#### frmProductList.cs
```csharp
using System;
using System.Windows.Forms;
using Northwind.BLL;
using Northwind.DTO;

namespace Northwind.GUI.Products
{
    public partial class frmProductList : Form
    {
        private ProductBLL productBLL = new ProductBLL();

        public frmProductList()
        {
            InitializeComponent();
        }

        private void frmProductList_Load(object sender, EventArgs e)
        {
            LoadData();
            LoadCategories();
        }

        private void LoadData()
        {
            try
            {
                var list = productBLL.LayDanhSach();
                dgvProducts.DataSource = list;

                // Format columns
                dgvProducts.Columns["ProductID"].HeaderText = "Mã SP";
                dgvProducts.Columns["ProductName"].HeaderText = "Tên sản phẩm";
                dgvProducts.Columns["CategoryName"].HeaderText = "Danh mục";
                dgvProducts.Columns["SupplierName"].HeaderText = "Nhà cung cấp";
                dgvProducts.Columns["UnitPrice"].HeaderText = "Giá";
                dgvProducts.Columns["UnitPrice"].DefaultCellStyle.Format = "N0";
                dgvProducts.Columns["UnitsInStock"].HeaderText = "Tồn kho";
                dgvProducts.Columns["Discontinued"].HeaderText = "Ngừng KD";

                // Hide unnecessary columns
                dgvProducts.Columns["SupplierID"].Visible = false;
                dgvProducts.Columns["CategoryID"].Visible = false;
                dgvProducts.Columns["QuantityPerUnit"].Visible = false;
                dgvProducts.Columns["UnitsOnOrder"].Visible = false;
                dgvProducts.Columns["ReorderLevel"].Visible = false;
            }
            catch (Exception ex)
            {
                MessageBox.Show(ex.Message, "Lỗi",
                    MessageBoxButtons.OK, MessageBoxIcon.Error);
            }
        }

        private void btnAdd_Click(object sender, EventArgs e)
        {
            frmProductDetail frm = new frmProductDetail();
            if (frm.ShowDialog() == DialogResult.OK)
            {
                LoadData();
            }
        }

        private void btnEdit_Click(object sender, EventArgs e)
        {
            if (dgvProducts.CurrentRow == null)
            {
                MessageBox.Show("Vui lòng chọn sản phẩm cần sửa");
                return;
            }

            int productID = (int)dgvProducts.CurrentRow.Cells["ProductID"].Value;
            frmProductDetail frm = new frmProductDetail(productID);
            if (frm.ShowDialog() == DialogResult.OK)
            {
                LoadData();
            }
        }

        private void btnDelete_Click(object sender, EventArgs e)
        {
            if (dgvProducts.CurrentRow == null)
            {
                MessageBox.Show("Vui lòng chọn sản phẩm cần xóa");
                return;
            }

            if (MessageBox.Show("Bạn có chắc muốn xóa sản phẩm này?", "Xác nhận",
                MessageBoxButtons.YesNo, MessageBoxIcon.Question) == DialogResult.Yes)
            {
                try
                {
                    int productID = (int)dgvProducts.CurrentRow.Cells["ProductID"].Value;
                    if (productBLL.Xoa(productID))
                    {
                        MessageBox.Show("Xóa thành công");
                        LoadData();
                    }
                }
                catch (Exception ex)
                {
                    MessageBox.Show(ex.Message, "Lỗi",
                        MessageBoxButtons.OK, MessageBoxIcon.Error);
                }
            }
        }

        private void txtSearch_TextChanged(object sender, EventArgs e)
        {
            try
            {
                var list = productBLL.TimKiem(txtSearch.Text);
                dgvProducts.DataSource = list;
            }
            catch (Exception ex)
            {
                MessageBox.Show(ex.Message, "Lỗi",
                    MessageBoxButtons.OK, MessageBoxIcon.Error);
            }
        }
    }
}
```

#### frmProductDetail.cs
```csharp
using System;
using System.Windows.Forms;
using Northwind.BLL;
using Northwind.DTO;

namespace Northwind.GUI.Products
{
    public partial class frmProductDetail : Form
    {
        private ProductBLL productBLL = new ProductBLL();
        private CategoryBLL categoryBLL = new CategoryBLL();
        private SupplierBLL supplierBLL = new SupplierBLL();
        private int? productID;

        public frmProductDetail()
        {
            InitializeComponent();
            productID = null;
        }

        public frmProductDetail(int id)
        {
            InitializeComponent();
            productID = id;
        }

        private void frmProductDetail_Load(object sender, EventArgs e)
        {
            LoadComboBoxes();

            if (productID.HasValue)
            {
                LoadProductData();
                this.Text = "Sửa sản phẩm";
            }
            else
            {
                this.Text = "Thêm sản phẩm mới";
            }
        }

        private void LoadComboBoxes()
        {
            // Load categories
            cboCategory.DataSource = categoryBLL.LayDanhSach();
            cboCategory.DisplayMember = "CategoryName";
            cboCategory.ValueMember = "CategoryID";

            // Load suppliers
            cboSupplier.DataSource = supplierBLL.LayDanhSach();
            cboSupplier.DisplayMember = "CompanyName";
            cboSupplier.ValueMember = "SupplierID";
        }

        private void LoadProductData()
        {
            try
            {
                var product = productBLL.TimTheoMa(productID.Value);
                if (product != null)
                {
                    txtProductName.Text = product.ProductName;
                    cboCategory.SelectedValue = product.CategoryID;
                    cboSupplier.SelectedValue = product.SupplierID;
                    txtQuantityPerUnit.Text = product.QuantityPerUnit;
                    nudUnitPrice.Value = product.UnitPrice ?? 0;
                    nudUnitsInStock.Value = product.UnitsInStock ?? 0;
                    nudUnitsOnOrder.Value = product.UnitsOnOrder ?? 0;
                    nudReorderLevel.Value = product.ReorderLevel ?? 0;
                    chkDiscontinued.Checked = product.Discontinued;
                }
            }
            catch (Exception ex)
            {
                MessageBox.Show(ex.Message, "Lỗi",
                    MessageBoxButtons.OK, MessageBoxIcon.Error);
            }
        }

        private void btnSave_Click(object sender, EventArgs e)
        {
            try
            {
                var product = new ProductDTO
                {
                    ProductName = txtProductName.Text.Trim(),
                    CategoryID = (int?)cboCategory.SelectedValue,
                    SupplierID = (int?)cboSupplier.SelectedValue,
                    QuantityPerUnit = txtQuantityPerUnit.Text.Trim(),
                    UnitPrice = nudUnitPrice.Value,
                    UnitsInStock = (short)nudUnitsInStock.Value,
                    UnitsOnOrder = (short)nudUnitsOnOrder.Value,
                    ReorderLevel = (short)nudReorderLevel.Value,
                    Discontinued = chkDiscontinued.Checked
                };

                bool success;
                if (productID.HasValue)
                {
                    product.ProductID = productID.Value;
                    success = productBLL.CapNhat(product);
                }
                else
                {
                    success = productBLL.Them(product);
                }

                if (success)
                {
                    MessageBox.Show("Lưu thành công");
                    this.DialogResult = DialogResult.OK;
                    this.Close();
                }
            }
            catch (Exception ex)
            {
                MessageBox.Show(ex.Message, "Lỗi",
                    MessageBoxButtons.OK, MessageBoxIcon.Error);
            }
        }

        private void btnCancel_Click(object sender, EventArgs e)
        {
            this.Close();
        }
    }
}
```

---

## 4. LUỒNG DỮ LIỆU (DATA FLOW)

```
┌──────────┐
│  USER    │
└────┬─────┘
     │
     ▼
┌─────────────────┐
│   GUI Layer     │  - frmProductList
│   (WinForms)    │  - Hiển thị DataGridView
└────┬────────────┘  - Xử lý user events
     │
     │ Call: productBLL.LayDanhSach()
     ▼
┌─────────────────┐
│   BLL Layer     │  - ProductBLL
│ (Business Logic)│  - Validation
└────┬────────────┘  - Exception handling
     │
     │ Call: productDAL.LayDanhSach()
     ▼
┌─────────────────┐
│   DAL Layer     │  - ProductDAL
│ (Data Access)   │  - SqlConnection
└────┬────────────┘  - SqlCommand, SqlDataReader
     │
     │ SQL Query
     ▼
┌─────────────────┐
│   DATABASE      │  - SQL Server
│   (Northwind)   │  - Tables: Products, Categories, etc.
└─────────────────┘
```

---

## 5. APP.CONFIG

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <connectionStrings>
    <add name="NorthwindDB"
         connectionString="Data Source=.;Initial Catalog=Northwind;Integrated Security=True"
         providerName="System.Data.SqlClient" />
  </connectionStrings>

  <appSettings>
    <add key="PageSize" value="50" />
    <add key="CompanyName" value="Northwind Traders" />
  </appSettings>
</configuration>
```

---

## 6. BEST PRACTICES

### 6.1. Exception Handling
```csharp
try
{
    // Code
}
catch (SqlException sqlEx)
{
    // Log SQL errors
    throw new Exception("Lỗi cơ sở dữ liệu: " + sqlEx.Message);
}
catch (Exception ex)
{
    // Log general errors
    throw new Exception("Lỗi: " + ex.Message);
}
```

### 6.2. Using Statement
```csharp
using (SqlConnection conn = DatabaseHelper.GetConnection())
{
    // Connection tự động đóng khi ra khỏi block
}
```

### 6.3. Parameterized Queries
```csharp
cmd.Parameters.AddWithValue("@ProductID", productID);
// KHÔNG BAO GIỜ: query = "SELECT * FROM Products WHERE ProductID = " + productID;
```

### 6.4. Nullable Types
```csharp
cmd.Parameters.AddWithValue("@SupplierID",
    (object)product.SupplierID ?? DBNull.Value);
```

---

## 7. SECURITY

1. **SQL Injection Prevention:** Luôn dùng parameterized queries
2. **Connection String Security:** Mã hóa connection string trong App.config
3. **Input Validation:** Validate tất cả input từ user
4. **Exception Messages:** Không expose chi tiết lỗi database cho user
5. **Transaction:** Dùng transaction cho các thao tác phức tạp

---

## 8. PERFORMANCE OPTIMIZATION

1. **Connection Pooling:** Tự động được SQL Server xử lý
2. **DataReader vs DataSet:** Dùng DataReader cho read-only, DataSet cho binding
3. **Lazy Loading:** Chỉ load dữ liệu khi cần
4. **Indexing:** Đảm bảo database có index phù hợp
5. **Stored Procedures:** Có thể dùng cho các query phức tạp

---

## 9. TESTING

### Unit Testing (NUnit/MSTest)
```csharp
[TestMethod]
public void TestThemSanPham_Valid()
{
    // Arrange
    var product = new ProductDTO
    {
        ProductName = "Test Product",
        UnitPrice = 100
    };

    // Act
    var result = productBLL.Them(product);

    // Assert
    Assert.IsTrue(result);
}

[TestMethod]
[ExpectedException(typeof(Exception))]
public void TestThemSanPham_InvalidName()
{
    // Arrange
    var product = new ProductDTO
    {
        ProductName = "", // Invalid
        UnitPrice = 100
    };

    // Act
    productBLL.Them(product);
}
```

---

## 10. DEPLOYMENT

1. **Build Release:** Build project ở chế độ Release
2. **Setup Project:** Tạo installer với InstallShield hoặc WiX
3. **Database Script:** Chuẩn bị script tạo database và data
4. **Config File:** Hướng dẫn user config connection string
5. **.NET Framework:** Đảm bảo máy client có .NET Framework phù hợp

---

## 11. TỔNG KẾT

### Ưu điểm của kiến trúc này:
- ✅ Tách biệt rõ ràng các concerns
- ✅ Dễ bảo trì và mở rộng
- ✅ Code reusability cao
- ✅ Dễ testing
- ✅ Performance tốt với ADO.NET

### Nhược điểm:
- ❌ Nhiều code boilerplate
- ❌ Không hỗ trợ async/await tốt
- ❌ Khó implement complex relationships

### Khi nào dùng:
- ✅ Ứng dụng desktop đơn giản đến trung bình
- ✅ Cần hiệu suất cao
- ✅ Database schema đơn giản
- ✅ Team nhỏ, timeline ngắn
