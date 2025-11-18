# KẾ HOẠCH PHÁT TRIỂN ỨNG DỤNG QUẢN LÝ NORTHWIND

**Công nghệ sử dụng:** .NET 10 WinForm C# + SQL Server (Tuần 1-3), Entity Framework Core 10 + ASP.NET Core 10 (Tuần 4-7)
**Cơ sở dữ liệu:** Northwind Database
**Thời gian thực hiện:** 7+ tuần (3 tuần cơ bản + 4 tuần nâng cao)

---

## KẾ HOẠCH BAN ĐẦU

### Mục tiêu
- Hiểu rõ yêu cầu dự án
- Xác định các chức năng chính của phần mềm
- Thiết kế cơ sở dữ liệu (CSDL)

### Nội dung thực hiện

#### 1. Phân tích yêu cầu
- Nghiên cứu mô hình kinh doanh Northwind (công ty thương mại xuất nhập khẩu)
- Xác định các đối tượng chính cần quản lý:
  - **Sản phẩm (Products)**: Quản lý hàng hóa kinh doanh
  - **Khách hàng (Customers)**: Quản lý thông tin khách hàng
  - **Đơn hàng (Orders)**: Quản lý đơn đặt hàng
  - **Nhân viên (Employees)**: Quản lý nhân viên bán hàng
  - **Nhà cung cấp (Suppliers)**: Quản lý nguồn hàng
  - **Danh mục (Categories)**: Phân loại sản phẩm
  - **Đơn vị vận chuyển (Shippers)**: Quản lý đối tác giao hàng

#### 2. Thiết kế sơ đồ ERD
- **Các bảng chính:**
  - `Suppliers`: Nhà cung cấp (SupplierID, CompanyName, ContactName, Address, City, Country, Phone)
  - `Categories`: Danh mục sản phẩm (CategoryID, CategoryName, Description)
  - `Products`: Sản phẩm (ProductID, ProductName, SupplierID, CategoryID, QuantityPerUnit, UnitPrice, UnitsInStock)
  - `Customers`: Khách hàng (CustomerID, CompanyName, ContactName, Address, City, Country, Phone)
  - `Employees`: Nhân viên (EmployeeID, LastName, FirstName, BirthDate, HireDate, Address, City, Country)
  - `Shippers`: Đơn vị vận chuyển (ShipperID, CompanyName, Phone)
  - `Orders`: Đơn hàng (OrderID, CustomerID, EmployeeID, OrderDate, RequiredDate, ShippedDate, ShipVia)
  - `Order Details`: Chi tiết đơn hàng (OrderID, ProductID, UnitPrice, Quantity, Discount)

- **Mối quan hệ:**
  - Products N-1 Categories
  - Products N-1 Suppliers
  - Orders N-1 Customers
  - Orders N-1 Employees
  - Orders N-1 Shippers
  - Order Details N-1 Orders
  - Order Details N-1 Products

#### 3. Thiết kế kiến trúc phần mềm
- **3-Layer Architecture:**
  - **DAL (Data Access Layer)**: Lớp truy xuất dữ liệu
  - **BLL (Business Logic Layer)**: Lớp xử lý nghiệp vụ
  - **GUI (Presentation Layer)**: Lớp giao diện người dùng

#### 4. Sản phẩm đầu ra
- ✅ Sơ đồ ERD hoàn chỉnh
- ✅ Danh sách chức năng chi tiết
- ✅ Thiết kế kiến trúc hệ thống
- ✅ Database Northwind đã được cài đặt trên SQL Server

---

## KẾ HOẠCH TUẦN 1

### Mục tiêu
- Xây dựng lớp kết nối và xử lý CSDL (DAL)
- Tạo giao diện và chức năng CRUD cơ bản
- Hoàn thiện module **Quản lý Sản phẩm (Products)**

### Nội dung thực hiện

#### 1. Xây dựng lớp Data Access Layer (DAL)

**a. Tạo lớp kết nối database**
```csharp
// DatabaseConnection.cs
- Chuỗi kết nối SQL Server
- Phương thức mở/đóng kết nối
- Xử lý exception
```

**b. Tạo lớp ProductDAL**
```csharp
// ProductDAL.cs
- GetAllProducts(): Lấy danh sách tất cả sản phẩm
- GetProductById(int productId): Lấy sản phẩm theo ID
- InsertProduct(Product product): Thêm sản phẩm mới
- UpdateProduct(Product product): Cập nhật thông tin sản phẩm
- DeleteProduct(int productId): Xóa sản phẩm
- GetProductsByCategory(int categoryId): Lấy sản phẩm theo danh mục
```

**c. Tạo lớp CategoryDAL**
```csharp
// CategoryDAL.cs
- GetAllCategories(): Lấy danh sách danh mục
- InsertCategory(Category category): Thêm danh mục
- UpdateCategory(Category category): Cập nhật danh mục
- DeleteCategory(int categoryId): Xóa danh mục
```

**d. Tạo lớp SupplierDAL**
```csharp
// SupplierDAL.cs
- GetAllSuppliers(): Lấy danh sách nhà cung cấp
- InsertSupplier(Supplier supplier): Thêm nhà cung cấp
- UpdateSupplier(Supplier supplier): Cập nhật nhà cung cấp
- DeleteSupplier(int supplierId): Xóa nhà cung cấp
```

#### 2. Xây dựng lớp Business Logic Layer (BLL)

**ProductBLL.cs**
```csharp
- Kiểm tra tính hợp lệ dữ liệu (validation)
- Xử lý nghiệp vụ: Kiểm tra tồn kho, giá bán
- Gọi các phương thức từ DAL
```

#### 3. Xây dựng giao diện Quản lý Sản phẩm

**Form: frmProductManagement**
- **DataGridView**: Hiển thị danh sách sản phẩm
  - Columns: ProductID, ProductName, Category, Supplier, UnitPrice, UnitsInStock, Discontinued

- **Panel nhập liệu:**
  - TextBox: ProductID (readonly), ProductName, UnitPrice, UnitsInStock, QuantityPerUnit
  - ComboBox: CategoryID, SupplierID
  - CheckBox: Discontinued

- **Buttons:**
  - btnAdd: Thêm sản phẩm mới
  - btnEdit: Sửa thông tin sản phẩm
  - btnDelete: Xóa sản phẩm
  - btnSave: Lưu thay đổi
  - btnCancel: Hủy thao tác
  - btnRefresh: Làm mới danh sách

#### 4. Xây dựng form phụ

**frmCategoryManagement**: Quản lý danh mục sản phẩm
- Thêm/Sửa/Xóa danh mục
- Hiển thị mô tả danh mục

**frmSupplierManagement**: Quản lý nhà cung cấp
- Thêm/Sửa/Xóa nhà cung cấp
- Hiển thị thông tin liên hệ

#### 5. Chức năng cần hoàn thiện

- ✅ CRUD đầy đủ cho Products
- ✅ CRUD đầy đủ cho Categories
- ✅ CRUD đầy đủ cho Suppliers
- ✅ Validation dữ liệu đầu vào
- ✅ Xử lý exception và hiển thị thông báo lỗi
- ✅ Load dữ liệu ComboBox (Categories, Suppliers)

#### 6. Kiểm thử
- Test các chức năng thêm/sửa/xóa
- Test validation dữ liệu
- Test xử lý lỗi khi xóa có ràng buộc khóa ngoại

---

## KẾ HOẠCH TUẦN 2

### Mục tiêu
- Xây dựng module **Quản lý Đơn hàng (Orders)**
- Thêm chức năng **Tìm kiếm nâng cao**
- Cập nhật ComboBox động giữa các form

### Nội dung thực hiện

#### 1. Xây dựng lớp DAL cho Orders

**OrderDAL.cs**
```csharp
- GetAllOrders(): Lấy tất cả đơn hàng
- GetOrderById(int orderId): Lấy đơn hàng theo ID
- GetOrdersByCustomer(string customerId): Lấy đơn hàng theo khách hàng
- GetOrdersByEmployee(int employeeId): Lấy đơn hàng theo nhân viên
- GetOrdersByDate(DateTime fromDate, DateTime toDate): Lấy đơn hàng theo khoảng thời gian
- InsertOrder(Order order): Thêm đơn hàng mới
- UpdateOrder(Order order): Cập nhật đơn hàng
- DeleteOrder(int orderId): Xóa đơn hàng
```

**OrderDetailsDAL.cs**
```csharp
- GetOrderDetailsByOrderId(int orderId): Lấy chi tiết đơn hàng
- InsertOrderDetail(OrderDetail detail): Thêm chi tiết đơn hàng
- UpdateOrderDetail(OrderDetail detail): Cập nhật chi tiết
- DeleteOrderDetail(int orderId, int productId): Xóa chi tiết
- CalculateOrderTotal(int orderId): Tính tổng tiền đơn hàng
```

**CustomerDAL.cs**
```csharp
- GetAllCustomers(): Lấy danh sách khách hàng
- GetCustomerById(string customerId): Lấy khách hàng theo ID
- InsertCustomer(Customer customer): Thêm khách hàng
- UpdateCustomer(Customer customer): Cập nhật khách hàng
- DeleteCustomer(string customerId): Xóa khách hàng
- SearchCustomers(string keyword): Tìm kiếm khách hàng
```

**EmployeeDAL.cs**
```csharp
- GetAllEmployees(): Lấy danh sách nhân viên
- GetEmployeeById(int employeeId): Lấy nhân viên theo ID
- InsertEmployee(Employee employee): Thêm nhân viên
- UpdateEmployee(Employee employee): Cập nhật nhân viên
- DeleteEmployee(int employeeId): Xóa nhân viên
```

#### 2. Xây dựng giao diện Quản lý Đơn hàng

**Form: frmOrderManagement**
- **DataGridView Orders**: Hiển thị danh sách đơn hàng
  - Columns: OrderID, CustomerName, EmployeeName, OrderDate, RequiredDate, ShippedDate, ShipperName, TotalAmount

- **DataGridView OrderDetails**: Hiển thị chi tiết đơn hàng đã chọn
  - Columns: ProductName, UnitPrice, Quantity, Discount, Total

- **Panel thông tin đơn hàng:**
  - TextBox: OrderID (readonly)
  - ComboBox: CustomerID, EmployeeID, ShipVia (Shippers)
  - DateTimePicker: OrderDate, RequiredDate, ShippedDate
  - TextBox: ShipAddress, ShipCity, ShipCountry

- **Panel chi tiết đơn hàng:**
  - ComboBox: ProductID
  - NumericUpDown: Quantity, Discount
  - TextBox: UnitPrice (readonly), Total (readonly)
  - Button: btnAddProduct, btnRemoveProduct

- **Buttons:**
  - btnNewOrder: Tạo đơn hàng mới
  - btnSaveOrder: Lưu đơn hàng
  - btnDeleteOrder: Xóa đơn hàng
  - btnPrint: In đơn hàng (chuẩn bị cho tuần 3)

#### 3. Xây dựng chức năng Tìm kiếm

**a. Tìm kiếm Sản phẩm (frmProductManagement)**
- Panel tìm kiếm:
  - TextBox: txtSearchProductName
  - ComboBox: cboSearchCategory, cboSearchSupplier
  - NumericUpDown: numPriceFrom, numPriceTo
  - CheckBox: chkInStock (Chỉ hiển thị hàng còn tồn kho)
  - Button: btnSearch, btnClearSearch

**b. Tìm kiếm Đơn hàng (frmOrderManagement)**
- Panel tìm kiếm:
  - TextBox: txtSearchOrderID
  - ComboBox: cboSearchCustomer, cboSearchEmployee
  - DateTimePicker: dtpFromDate, dtpToDate
  - ComboBox: cboOrderStatus (All, Pending, Shipped, Delivered)
  - Button: btnSearch, btnClearSearch

**c. Tìm kiếm Khách hàng (frmCustomerManagement)**
- Panel tìm kiếm:
  - TextBox: txtSearchCustomerName, txtSearchCountry, txtSearchCity
  - Button: btnSearch, btnClearSearch

#### 4. Cập nhật ComboBox động

**a. Tạo lớp tiện ích**
```csharp
// ComboBoxHelper.cs
- LoadCategories(ComboBox cbo): Load danh mục
- LoadSuppliers(ComboBox cbo): Load nhà cung cấp
- LoadCustomers(ComboBox cbo): Load khách hàng
- LoadEmployees(ComboBox cbo): Load nhân viên
- LoadShippers(ComboBox cbo): Load đơn vị vận chuyển
- LoadProducts(ComboBox cbo): Load sản phẩm
- RefreshComboBox(ComboBox cbo, string type): Làm mới ComboBox
```

**b. Cập nhật tự động**
- Sau khi thêm/sửa/xóa Category → Refresh ComboBox trong frmProductManagement
- Sau khi thêm/sửa/xóa Supplier → Refresh ComboBox trong frmProductManagement
- Sau khi thêm Customer → Refresh ComboBox trong frmOrderManagement

#### 5. Xây dựng form phụ

**frmCustomerManagement**: Quản lý khách hàng
- CRUD khách hàng
- Tìm kiếm khách hàng
- Hiển thị lịch sử đơn hàng của khách hàng

**frmEmployeeManagement**: Quản lý nhân viên
- CRUD nhân viên
- Hiển thị ảnh nhân viên
- Hiển thị thông tin cấp trên (ReportsTo)

#### 6. Chức năng cần hoàn thiện

- ✅ CRUD đầy đủ cho Orders và Order Details
- ✅ CRUD đầy đủ cho Customers
- ✅ CRUD đầy đủ cho Employees
- ✅ Tính toán tự động tổng tiền đơn hàng
- ✅ Tìm kiếm nâng cao cho tất cả modules
- ✅ Cập nhật ComboBox động
- ✅ Validation nghiệp vụ (số lượng tồn kho, giá trị đơn hàng)

#### 7. Kiểm thử
- Test tạo đơn hàng với nhiều sản phẩm
- Test tìm kiếm theo nhiều tiêu chí
- Test cập nhật ComboBox sau khi thêm/sửa/xóa
- Test xử lý đơn hàng khi xóa sản phẩm đã có trong Order Details

---

## KẾ HOẠCH TUẦN 3

### Mục tiêu
- Xây dựng chức năng **Thống kê**
- Xây dựng chức năng **Xuất báo cáo Excel**
- In danh sách và hóa đơn

### Nội dung thực hiện

#### 1. Xây dựng lớp DAL cho Thống kê

**StatisticsDAL.cs**
```csharp
// Thống kê sản phẩm
- GetTopSellingProducts(int top): Sản phẩm bán chạy nhất
- GetProductsByStockStatus(): Thống kê tồn kho (hết hàng, sắp hết, còn nhiều)
- GetRevenueByProduct(DateTime fromDate, DateTime toDate): Doanh thu theo sản phẩm

// Thống kê đơn hàng
- GetTotalOrdersByMonth(int year): Thống kê đơn hàng theo tháng
- GetRevenueByMonth(int year): Doanh thu theo tháng
- GetOrdersByStatus(): Thống kê đơn hàng theo trạng thái
- GetTotalRevenueByDateRange(DateTime fromDate, DateTime toDate): Tổng doanh thu

// Thống kê khách hàng
- GetTopCustomers(int top): Khách hàng mua nhiều nhất
- GetCustomersByCountry(): Phân bố khách hàng theo quốc gia
- GetNewCustomersByMonth(int year): Khách hàng mới theo tháng

// Thống kê nhân viên
- GetEmployeePerformance(DateTime fromDate, DateTime toDate): Hiệu suất nhân viên
- GetTopEmployeesBySales(int top): Nhân viên bán hàng xuất sắc
```

#### 2. Xây dựng giao diện Thống kê

**Form: frmStatistics**

**Tab 1: Thống kê Sản phẩm**
- Chart: Top 10 sản phẩm bán chạy (Column Chart)
- Chart: Tình trạng tồn kho (Pie Chart)
- DataGridView: Chi tiết doanh thu theo sản phẩm
- DateTimePicker: Chọn khoảng thời gian
- Button: btnViewReport, btnExport

**Tab 2: Thống kê Doanh thu**
- Chart: Doanh thu theo tháng (Line Chart)
- Chart: So sánh doanh thu các năm (Bar Chart)
- Panel hiển thị số liệu:
  - Label: Tổng doanh thu, Trung bình đơn hàng, Số đơn hàng
- ComboBox: cboYear (Chọn năm)
- DateTimePicker: Từ ngày - Đến ngày
- Button: btnViewReport, btnExport

**Tab 3: Thống kê Khách hàng**
- Chart: Top 10 khách hàng (Bar Chart)
- Chart: Phân bố khách hàng theo quốc gia (Pie Chart)
- DataGridView: Danh sách khách hàng và doanh thu
- Button: btnViewReport, btnExport

**Tab 4: Thống kê Nhân viên**
- Chart: Hiệu suất bán hàng nhân viên (Column Chart)
- DataGridView: Chi tiết doanh số từng nhân viên
- DateTimePicker: Chọn khoảng thời gian
- Button: btnViewReport, btnExport

#### 3. Xuất báo cáo Excel

**Sử dụng thư viện:** EPPlus hoặc ClosedXML

**Tạo lớp ExcelExporter.cs**
```csharp
// Xuất danh sách
- ExportProductList(List<Product> products, string filePath): Xuất DS sản phẩm
- ExportCustomerList(List<Customer> customers, string filePath): Xuất DS khách hàng
- ExportOrderList(List<Order> orders, string filePath): Xuất DS đơn hàng

// Xuất báo cáo thống kê
- ExportRevenueReport(DataTable data, string filePath): Báo cáo doanh thu
- ExportProductStatistics(DataTable data, string filePath): Thống kê sản phẩm
- ExportCustomerStatistics(DataTable data, string filePath): Thống kê khách hàng
- ExportEmployeePerformance(DataTable data, string filePath): Hiệu suất nhân viên

// Định dạng Excel
- FormatHeader(): Định dạng tiêu đề (màu, font, border)
- AddChart(): Thêm biểu đồ vào Excel
- AutoFitColumns(): Tự động căn chỉnh độ rộng cột
```

**Chức năng xuất Excel trong các form:**
- frmProductManagement → Xuất danh sách sản phẩm
- frmOrderManagement → Xuất danh sách đơn hàng
- frmCustomerManagement → Xuất danh sách khách hàng
- frmStatistics → Xuất báo cáo thống kê kèm biểu đồ

#### 4. In danh sách và hóa đơn

**a. In danh sách**
- Sử dụng PrintDocument, PrintPreviewDialog
- Tạo lớp PrintHelper.cs:
  - PrintProductList(): In danh sách sản phẩm
  - PrintCustomerList(): In danh sách khách hàng
  - PrintOrderList(): In danh sách đơn hàng

**b. In hóa đơn**
- Form: frmPrintInvoice
  - Hiển thị preview hóa đơn
  - Thông tin: Logo công ty, thông tin khách hàng, chi tiết sản phẩm, tổng tiền
  - Button: btnPrint, btnExportPDF

- Tạo template hóa đơn:
  - Header: Tên công ty, địa chỉ, số điện thoại
  - Body: Thông tin đơn hàng, bảng chi tiết sản phẩm
  - Footer: Tổng tiền, chữ ký

#### 5. Dashboard tổng quan

**Form: frmDashboard (Form chính)**
- Panel thống kê nhanh:
  - Tổng sản phẩm
  - Tổng khách hàng
  - Tổng đơn hàng tháng này
  - Doanh thu tháng này

- Chart nhỏ: Doanh thu 7 ngày gần nhất
- Danh sách: Đơn hàng mới nhất (5 đơn)
- Danh sách: Sản phẩm sắp hết hàng

- Menu:
  - Quản lý Sản phẩm
  - Quản lý Đơn hàng
  - Quản lý Khách hàng
  - Quản lý Nhân viên
  - Thống kê & Báo cáo
  - Cài đặt

#### 6. Chức năng bổ sung

**a. Cài đặt ứng dụng**
- Form: frmSettings
  - Cấu hình kết nối database
  - Thông tin công ty (cho in hóa đơn)
  - Sao lưu và phục hồi database
  - Quản lý người dùng (nếu có phân quyền)

**b. Tối ưu hóa**
- Sử dụng Stored Procedures cho các truy vấn phức tạp
- Cache dữ liệu ComboBox
- Phân trang cho DataGridView (nếu dữ liệu lớn)
- Loading indicator cho các thao tác chậm

#### 7. Chức năng cần hoàn thiện

- ✅ Tất cả các biểu đồ thống kê
- ✅ Xuất Excel đầy đủ cho tất cả modules
- ✅ In danh sách và hóa đơn
- ✅ Dashboard tổng quan
- ✅ Form cài đặt ứng dụng
- ✅ Tối ưu hóa hiệu năng

#### 8. Kiểm thử tổng thể

- Test tất cả chức năng thống kê với dữ liệu thực
- Test xuất Excel và kiểm tra format
- Test in ấn trên nhiều khổ giấy khác nhau
- Test hiệu năng với dữ liệu lớn
- Test tích hợp giữa các modules

---

# PHẦN 2: NÂNG CAO (Tuần 4-7)

*Dựa trên đề cương môn học Lập trình Cơ sở Dữ liệu (ITEC3406)*

---

## KẾ HOẠCH TUẦN 4

### Mục tiêu
- Nâng cấp từ **ADO.NET** lên **Entity Framework**
- Áp dụng **Database First approach**
- Tối ưu hóa code với **DTO Pattern**
- Refactor toàn bộ DAL layer

### Nội dung thực hiện

#### 1. Cài đặt Entity Framework

**a. Cài đặt packages**
```bash
# .NET 10 - Entity Framework Core 10
dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 10.0.0
dotnet add package Microsoft.EntityFrameworkCore.Tools --version 10.0.0
dotnet add package Microsoft.EntityFrameworkCore.Design --version 10.0.0

# Hoặc dùng Package Manager Console trong Visual Studio:
Install-Package Microsoft.EntityFrameworkCore.SqlServer -Version 10.0.0
Install-Package Microsoft.EntityFrameworkCore.Tools -Version 10.0.0
Install-Package Microsoft.EntityFrameworkCore.Design -Version 10.0.0
```

**b. Tạo DbContext từ Database (Database First)**
```bash
# .NET 10 - Scaffold command
dotnet ef dbcontext scaffold "Server=.;Database=Northwind;Trusted_Connection=True;TrustServerCertificate=True" Microsoft.EntityFrameworkCore.SqlServer --output-dir Models --context NorthwindContext --force

# Hoặc trong Package Manager Console:
Scaffold-DbContext "Server=.;Database=Northwind;Trusted_Connection=True;TrustServerCertificate=True" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Context NorthwindContext -Force
```

**Kết quả:**
- Folder `Models/` chứa các entity classes: Product, Category, Supplier, Order, OrderDetail, Customer, Employee, Shipper
- File `NorthwindContext.cs`: DbContext với DbSet cho tất cả entities

#### 2. Tạo lớp DTO (Data Transfer Objects)

**Mục đích:**
- Tách biệt Entity models và Business models
- Tránh expose toàn bộ thông tin database
- Dễ dàng customize dữ liệu trả về

**ProductDTO.cs**
```csharp
public class ProductDTO
{
    public int ProductID { get; set; }
    public string ProductName { get; set; }
    public string CategoryName { get; set; }
    public string SupplierName { get; set; }
    public decimal? UnitPrice { get; set; }
    public short? UnitsInStock { get; set; }
    public bool Discontinued { get; set; }
}
```

**OrderDTO.cs**
```csharp
public class OrderDTO
{
    public int OrderID { get; set; }
    public string CustomerName { get; set; }
    public string EmployeeName { get; set; }
    public DateTime? OrderDate { get; set; }
    public DateTime? RequiredDate { get; set; }
    public DateTime? ShippedDate { get; set; }
    public string ShipperName { get; set; }
    public decimal TotalAmount { get; set; }
    public List<OrderDetailDTO> OrderDetails { get; set; }
}
```

#### 3. Refactor DAL với Entity Framework

**ProductRepository.cs** (thay thế ProductDAL)
```csharp
public class ProductRepository
{
    private readonly NorthwindContext _context;

    public ProductRepository()
    {
        _context = new NorthwindContext();
    }

    // CRUD Operations
    public List<Product> GetAllProducts()
    {
        return _context.Products
            .Include(p => p.Category)
            .Include(p => p.Supplier)
            .ToList();
    }

    public Product GetProductById(int id)
    {
        return _context.Products
            .Include(p => p.Category)
            .Include(p => p.Supplier)
            .FirstOrDefault(p => p.ProductID == id);
    }

    public void InsertProduct(Product product)
    {
        _context.Products.Add(product);
        _context.SaveChanges();
    }

    public void UpdateProduct(Product product)
    {
        _context.Entry(product).State = EntityState.Modified;
        _context.SaveChanges();
    }

    public void DeleteProduct(int id)
    {
        var product = _context.Products.Find(id);
        if (product != null)
        {
            _context.Products.Remove(product);
            _context.SaveChanges();
        }
    }

    // Advanced queries
    public List<Product> GetProductsByCategory(int categoryId)
    {
        return _context.Products
            .Where(p => p.CategoryID == categoryId)
            .ToList();
    }

    public List<Product> SearchProducts(string keyword)
    {
        return _context.Products
            .Where(p => p.ProductName.Contains(keyword))
            .ToList();
    }
}
```

**OrderRepository.cs** (thay thế OrderDAL)
```csharp
public class OrderRepository
{
    private readonly NorthwindContext _context;

    public List<Order> GetAllOrders()
    {
        return _context.Orders
            .Include(o => o.Customer)
            .Include(o => o.Employee)
            .Include(o => o.Shipper)
            .Include(o => o.OrderDetails)
                .ThenInclude(od => od.Product)
            .ToList();
    }

    public Order GetOrderById(int id)
    {
        return _context.Orders
            .Include(o => o.Customer)
            .Include(o => o.Employee)
            .Include(o => o.OrderDetails)
                .ThenInclude(od => od.Product)
            .FirstOrDefault(o => o.OrderID == id);
    }

    public void InsertOrder(Order order)
    {
        _context.Orders.Add(order);
        _context.SaveChanges();
    }

    public decimal CalculateOrderTotal(int orderId)
    {
        return _context.OrderDetails
            .Where(od => od.OrderID == orderId)
            .Sum(od => od.UnitPrice * od.Quantity * (1 - (decimal)od.Discount));
    }
}
```

#### 4. Tạo Service Layer (thay thế BLL)

**ProductService.cs**
```csharp
public class ProductService
{
    private readonly ProductRepository _productRepo;

    public ProductService()
    {
        _productRepo = new ProductRepository();
    }

    public List<ProductDTO> GetAllProducts()
    {
        var products = _productRepo.GetAllProducts();
        return products.Select(p => new ProductDTO
        {
            ProductID = p.ProductID,
            ProductName = p.ProductName,
            CategoryName = p.Category?.CategoryName,
            SupplierName = p.Supplier?.CompanyName,
            UnitPrice = p.UnitPrice,
            UnitsInStock = p.UnitsInStock,
            Discontinued = p.Discontinued
        }).ToList();
    }

    public bool AddProduct(Product product)
    {
        // Validation
        if (string.IsNullOrWhiteSpace(product.ProductName))
            throw new ArgumentException("Product name is required");

        if (product.UnitPrice < 0)
            throw new ArgumentException("Unit price cannot be negative");

        _productRepo.InsertProduct(product);
        return true;
    }
}
```

#### 5. Cập nhật WinForm để sử dụng EF

**frmProductManagement.cs**
```csharp
private ProductService _productService = new ProductService();

private void LoadProducts()
{
    var products = _productService.GetAllProducts();
    dgvProducts.DataSource = products;
}

private void btnAdd_Click(object sender, EventArgs e)
{
    var product = new Product
    {
        ProductName = txtProductName.Text,
        CategoryID = (int)cboCategoryID.SelectedValue,
        SupplierID = (int)cboSupplierID.SelectedValue,
        UnitPrice = decimal.Parse(txtUnitPrice.Text),
        UnitsInStock = short.Parse(txtUnitsInStock.Text)
    };

    _productService.AddProduct(product);
    LoadProducts();
}
```

#### 6. So sánh ADO.NET vs Entity Framework

**ADO.NET (Tuần 1-3):**
```csharp
// Phải viết SQL thủ công
string sql = "SELECT * FROM Products WHERE CategoryID = @CategoryID";
SqlCommand cmd = new SqlCommand(sql, conn);
cmd.Parameters.AddWithValue("@CategoryID", categoryId);
// ...đọc dữ liệu từ SqlDataReader
```

**Entity Framework (Tuần 4+):**
```csharp
// Sử dụng LINQ, type-safe
var products = _context.Products
    .Where(p => p.CategoryID == categoryId)
    .ToList();
```

#### 7. Migration và Database Updates

**Tạo Migration**
```bash
Add-Migration AddProductDescription
Update-Database
```

**Ví dụ thêm column mới:**
```csharp
// Migration file
public partial class AddProductDescription : Migration
{
    protected override void Up(MigrationBuilder migrationBuilder)
    {
        migrationBuilder.AddColumn<string>(
            name: "Description",
            table: "Products",
            maxLength: 500,
            nullable: true);
    }

    protected override void Down(MigrationBuilder migrationBuilder)
    {
        migrationBuilder.DropColumn(
            name: "Description",
            table: "Products");
    }
}
```

#### 8. Chức năng cần hoàn thiện

- ✅ Cài đặt Entity Framework Core
- ✅ Generate entities từ database (Database First)
- ✅ Tạo tất cả Repositories (Product, Order, Customer, Employee, Category, Supplier)
- ✅ Tạo DTOs cho tất cả entities
- ✅ Tạo Service layer với validation
- ✅ Cập nhật tất cả WinForm để sử dụng EF thay vì ADO.NET
- ✅ Eager Loading với Include() và ThenInclude()
- ✅ Testing và so sánh performance

#### 9. Kiểm thử

- Test tất cả CRUD operations với EF
- So sánh performance ADO.NET vs EF
- Test Eager Loading vs Lazy Loading
- Test Transaction handling
- Test với dữ liệu lớn

---

## KẾ HOẠCH TUẦN 5

### Mục tiêu
- Áp dụng **LINQ** (Language Integrated Query) toàn diện
- Tối ưu hóa với **Stored Procedures**
- Áp dụng **Repository Pattern** và **Unit of Work Pattern**
- Cải thiện performance và bảo mật

### Nội dung thực hiện

#### 1. LINQ to Entities - Truy vấn nâng cao

**a. LINQ Basic Queries**
```csharp
// Query Syntax
var products = from p in _context.Products
               where p.UnitPrice > 50
               orderby p.ProductName
               select p;

// Method Syntax (khuyến khích)
var products = _context.Products
    .Where(p => p.UnitPrice > 50)
    .OrderBy(p => p.ProductName)
    .ToList();
```

**b. LINQ Joins**
```csharp
// Inner Join
var productsWithCategory = _context.Products
    .Join(_context.Categories,
        p => p.CategoryID,
        c => c.CategoryID,
        (p, c) => new ProductDTO
        {
            ProductID = p.ProductID,
            ProductName = p.ProductName,
            CategoryName = c.CategoryName,
            UnitPrice = p.UnitPrice
        })
    .ToList();

// Group Join
var categoriesWithProducts = _context.Categories
    .GroupJoin(_context.Products,
        c => c.CategoryID,
        p => p.CategoryID,
        (c, products) => new
        {
            Category = c.CategoryName,
            ProductCount = products.Count(),
            Products = products.ToList()
        })
    .ToList();
```

**c. LINQ Aggregations**
```csharp
// Thống kê nâng cao
var statistics = new
{
    TotalProducts = _context.Products.Count(),
    AveragePrice = _context.Products.Average(p => p.UnitPrice),
    MaxPrice = _context.Products.Max(p => p.UnitPrice),
    MinPrice = _context.Products.Min(p => p.UnitPrice),
    TotalValue = _context.Products.Sum(p => p.UnitPrice * p.UnitsInStock)
};

// Group By
var productsByCategory = _context.Products
    .GroupBy(p => p.Category.CategoryName)
    .Select(g => new
    {
        Category = g.Key,
        Count = g.Count(),
        AveragePrice = g.Average(p => p.UnitPrice),
        TotalStock = g.Sum(p => p.UnitsInStock)
    })
    .ToList();
```

**d. LINQ Complex Queries**
```csharp
// Top 10 khách hàng mua nhiều nhất
var topCustomers = _context.Orders
    .GroupBy(o => o.Customer)
    .Select(g => new
    {
        CustomerName = g.Key.CompanyName,
        TotalOrders = g.Count(),
        TotalAmount = g.Sum(o => o.OrderDetails.Sum(od => od.UnitPrice * od.Quantity))
    })
    .OrderByDescending(x => x.TotalAmount)
    .Take(10)
    .ToList();

// Sản phẩm chưa bán được
var unsoldProducts = _context.Products
    .Where(p => !_context.OrderDetails.Any(od => od.ProductID == p.ProductID))
    .ToList();
```

#### 2. Stored Procedures trong Entity Framework

**a. Tạo Stored Procedures trong SQL Server**

```sql
-- SP_GetProductsByCategory
CREATE PROCEDURE SP_GetProductsByCategory
    @CategoryID INT
AS
BEGIN
    SELECT p.*, c.CategoryName, s.CompanyName as SupplierName
    FROM Products p
    LEFT JOIN Categories c ON p.CategoryID = c.CategoryID
    LEFT JOIN Suppliers s ON p.SupplierID = s.SupplierID
    WHERE p.CategoryID = @CategoryID
END
GO

-- SP_GetOrderStatistics
CREATE PROCEDURE SP_GetOrderStatistics
    @FromDate DATETIME,
    @ToDate DATETIME
AS
BEGIN
    SELECT
        COUNT(*) as TotalOrders,
        SUM(od.UnitPrice * od.Quantity * (1 - od.Discount)) as TotalRevenue,
        AVG(od.UnitPrice * od.Quantity) as AverageOrderValue
    FROM Orders o
    INNER JOIN [Order Details] od ON o.OrderID = od.OrderID
    WHERE o.OrderDate BETWEEN @FromDate AND @ToDate
END
GO

-- SP_InsertProduct
CREATE PROCEDURE SP_InsertProduct
    @ProductName NVARCHAR(40),
    @SupplierID INT,
    @CategoryID INT,
    @UnitPrice MONEY,
    @UnitsInStock SMALLINT,
    @ProductID INT OUTPUT
AS
BEGIN
    INSERT INTO Products (ProductName, SupplierID, CategoryID, UnitPrice, UnitsInStock)
    VALUES (@ProductName, @SupplierID, @CategoryID, @UnitPrice, @UnitsInStock)

    SET @ProductID = SCOPE_IDENTITY()
END
GO

-- SP_UpdateProductStock
CREATE PROCEDURE SP_UpdateProductStock
    @ProductID INT,
    @QuantityChange INT
AS
BEGIN
    UPDATE Products
    SET UnitsInStock = UnitsInStock + @QuantityChange
    WHERE ProductID = @ProductID
END
GO
```

**b. Gọi Stored Procedures trong Entity Framework**

```csharp
public class ProductRepository
{
    private readonly NorthwindContext _context;

    // Gọi SP trả về dữ liệu
    public List<Product> GetProductsByCategory(int categoryId)
    {
        var categoryIdParam = new SqlParameter("@CategoryID", categoryId);

        return _context.Products
            .FromSqlRaw("EXEC SP_GetProductsByCategory @CategoryID", categoryIdParam)
            .ToList();
    }

    // Gọi SP với OUTPUT parameter
    public int InsertProductSP(string productName, int supplierId, int categoryId, decimal unitPrice, short unitsInStock)
    {
        var productIdParam = new SqlParameter
        {
            ParameterName = "@ProductID",
            SqlDbType = SqlDbType.Int,
            Direction = ParameterDirection.Output
        };

        _context.Database.ExecuteSqlRaw(
            "EXEC SP_InsertProduct @ProductName, @SupplierID, @CategoryID, @UnitPrice, @UnitsInStock, @ProductID OUTPUT",
            new SqlParameter("@ProductName", productName),
            new SqlParameter("@SupplierID", supplierId),
            new SqlParameter("@CategoryID", categoryId),
            new SqlParameter("@UnitPrice", unitPrice),
            new SqlParameter("@UnitsInStock", unitsInStock),
            productIdParam
        );

        return (int)productIdParam.Value;
    }

    // Gọi SP không trả về dữ liệu
    public void UpdateProductStock(int productId, int quantityChange)
    {
        _context.Database.ExecuteSqlRaw(
            "EXEC SP_UpdateProductStock @ProductID, @QuantityChange",
            new SqlParameter("@ProductID", productId),
            new SqlParameter("@QuantityChange", quantityChange)
        );
    }
}
```

#### 3. Repository Pattern và Unit of Work

**a. Generic Repository**
```csharp
public interface IRepository<T> where T : class
{
    IEnumerable<T> GetAll();
    T GetById(object id);
    void Insert(T entity);
    void Update(T entity);
    void Delete(object id);
    IEnumerable<T> Find(Expression<Func<T, bool>> predicate);
}

public class Repository<T> : IRepository<T> where T : class
{
    private readonly NorthwindContext _context;
    private readonly DbSet<T> _dbSet;

    public Repository(NorthwindContext context)
    {
        _context = context;
        _dbSet = context.Set<T>();
    }

    public IEnumerable<T> GetAll()
    {
        return _dbSet.ToList();
    }

    public T GetById(object id)
    {
        return _dbSet.Find(id);
    }

    public void Insert(T entity)
    {
        _dbSet.Add(entity);
    }

    public void Update(T entity)
    {
        _dbSet.Attach(entity);
        _context.Entry(entity).State = EntityState.Modified;
    }

    public void Delete(object id)
    {
        T entity = _dbSet.Find(id);
        _dbSet.Remove(entity);
    }

    public IEnumerable<T> Find(Expression<Func<T, bool>> predicate)
    {
        return _dbSet.Where(predicate).ToList();
    }
}
```

**b. Unit of Work Pattern**
```csharp
public interface IUnitOfWork : IDisposable
{
    IRepository<Product> Products { get; }
    IRepository<Category> Categories { get; }
    IRepository<Order> Orders { get; }
    IRepository<Customer> Customers { get; }
    IRepository<Employee> Employees { get; }
    int SaveChanges();
}

public class UnitOfWork : IUnitOfWork
{
    private readonly NorthwindContext _context;

    private IRepository<Product> _products;
    private IRepository<Category> _categories;
    private IRepository<Order> _orders;
    private IRepository<Customer> _customers;
    private IRepository<Employee> _employees;

    public UnitOfWork(NorthwindContext context)
    {
        _context = context;
    }

    public IRepository<Product> Products
    {
        get { return _products ?? (_products = new Repository<Product>(_context)); }
    }

    public IRepository<Category> Categories
    {
        get { return _categories ?? (_categories = new Repository<Category>(_context)); }
    }

    public IRepository<Order> Orders
    {
        get { return _orders ?? (_orders = new Repository<Order>(_context)); }
    }

    public IRepository<Customer> Customers
    {
        get { return _customers ?? (_customers = new Repository<Customer>(_context)); }
    }

    public IRepository<Employee> Employees
    {
        get { return _employees ?? (_employees = new Repository<Employee>(_context)); }
    }

    public int SaveChanges()
    {
        return _context.SaveChanges();
    }

    public void Dispose()
    {
        _context.Dispose();
    }
}
```

**c. Sử dụng Unit of Work trong Service**
```csharp
public class ProductService
{
    private readonly IUnitOfWork _unitOfWork;

    public ProductService(IUnitOfWork unitOfWork)
    {
        _unitOfWork = unitOfWork;
    }

    public void TransferStock(int fromProductId, int toProductId, short quantity)
    {
        try
        {
            var fromProduct = _unitOfWork.Products.GetById(fromProductId);
            var toProduct = _unitOfWork.Products.GetById(toProductId);

            if (fromProduct.UnitsInStock < quantity)
                throw new InvalidOperationException("Insufficient stock");

            fromProduct.UnitsInStock -= quantity;
            toProduct.UnitsInStock += quantity;

            _unitOfWork.Products.Update(fromProduct);
            _unitOfWork.Products.Update(toProduct);

            _unitOfWork.SaveChanges(); // Transaction commit
        }
        catch
        {
            // Transaction rollback tự động
            throw;
        }
    }
}
```

#### 4. LINQ to XML - Xuất/Nhập dữ liệu XML

**a. Xuất dữ liệu ra XML**
```csharp
public void ExportProductsToXML(string filePath)
{
    var products = _context.Products.Include(p => p.Category).ToList();

    var xml = new XElement("Products",
        from p in products
        select new XElement("Product",
            new XElement("ProductID", p.ProductID),
            new XElement("ProductName", p.ProductName),
            new XElement("Category", p.Category?.CategoryName),
            new XElement("UnitPrice", p.UnitPrice),
            new XElement("UnitsInStock", p.UnitsInStock)
        )
    );

    xml.Save(filePath);
}
```

**b. Đọc dữ liệu từ XML**
```csharp
public List<ProductDTO> ImportProductsFromXML(string filePath)
{
    var xml = XElement.Load(filePath);

    var products = from p in xml.Elements("Product")
                   select new ProductDTO
                   {
                       ProductID = (int)p.Element("ProductID"),
                       ProductName = (string)p.Element("ProductName"),
                       CategoryName = (string)p.Element("Category"),
                       UnitPrice = (decimal)p.Element("UnitPrice"),
                       UnitsInStock = (short)p.Element("UnitsInStock")
                   };

    return products.ToList();
}
```

#### 5. Performance Optimization

**a. AsNoTracking() cho Read-Only queries**
```csharp
// Nhanh hơn 30-40% cho read-only
var products = _context.Products
    .AsNoTracking()
    .ToList();
```

**b. Pagination**
```csharp
public PagedResult<Product> GetProductsPaged(int pageNumber, int pageSize)
{
    var query = _context.Products.AsQueryable();

    var total = query.Count();
    var items = query
        .OrderBy(p => p.ProductName)
        .Skip((pageNumber - 1) * pageSize)
        .Take(pageSize)
        .ToList();

    return new PagedResult<Product>
    {
        Items = items,
        TotalCount = total,
        PageNumber = pageNumber,
        PageSize = pageSize
    };
}
```

**c. Compiled Queries**
```csharp
private static readonly Func<NorthwindContext, int, Product> GetProductByIdCompiled =
    EF.CompileQuery((NorthwindContext context, int id) =>
        context.Products.FirstOrDefault(p => p.ProductID == id));

public Product GetProductById(int id)
{
    return GetProductByIdCompiled(_context, id);
}
```

#### 6. Chức năng cần hoàn thiện

- ✅ Áp dụng LINQ queries cho tất cả modules
- ✅ Tạo Stored Procedures cho các thao tác phức tạp
- ✅ Implement Generic Repository Pattern
- ✅ Implement Unit of Work Pattern
- ✅ Xuất/Nhập dữ liệu XML
- ✅ Tối ưu hóa performance với AsNoTracking, Pagination
- ✅ Refactor toàn bộ Service layer

#### 7. Kiểm thử

- Test LINQ queries phức tạp
- Test Stored Procedures với nhiều tham số
- Test Unit of Work với transactions
- Test performance trước và sau optimization
- Test XML import/export với dữ liệu lớn

---

## KẾ HOẠCH TUẦN 6

### Mục tiêu
- Xây dựng **ASP.NET Core Web API** cho hệ thống Northwind
- Áp dụng **RESTful API** design principles
- Implement **JWT Authentication**
- Deploy lên **Azure** hoặc **IIS**

### Nội dung thực hiện

#### 1. Tạo ASP.NET Core Web API Project

**a. Cấu trúc project**
```
NorthwindAPI/
├── Controllers/
│   ├── ProductsController.cs
│   ├── OrdersController.cs
│   ├── CustomersController.cs
│   └── AuthController.cs
├── Models/
│   ├── DTOs/
│   └── Entities/
├── Services/
│   ├── IProductService.cs
│   └── ProductService.cs
├── Data/
│   └── NorthwindContext.cs
├── Middleware/
│   └── ErrorHandlingMiddleware.cs
├── appsettings.json
└── Program.cs
```

**b. Cấu hình appsettings.json**
```json
{
  "ConnectionStrings": {
    "NorthwindConnection": "Server=.;Database=Northwind;Trusted_Connection=True;TrustServerCertificate=True"
  },
  "Jwt": {
    "Key": "YourSuperSecretKeyHere123456789",
    "Issuer": "NorthwindAPI",
    "Audience": "NorthwindClient",
    "ExpireMinutes": 60
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information"
    }
  }
}
```

#### 2. Xây dựng RESTful API Controllers

**ProductsController.cs**
```csharp
[ApiController]
[Route("api/[controller]")]
public class ProductsController : ControllerBase
{
    private readonly IProductService _productService;

    public ProductsController(IProductService productService)
    {
        _productService = productService;
    }

    // GET: api/products
    [HttpGet]
    public async Task<ActionResult<IEnumerable<ProductDTO>>> GetProducts(
        [FromQuery] int pageNumber = 1,
        [FromQuery] int pageSize = 10,
        [FromQuery] string search = null)
    {
        var result = await _productService.GetProductsAsync(pageNumber, pageSize, search);

        Response.Headers.Add("X-Total-Count", result.TotalCount.ToString());
        Response.Headers.Add("X-Page-Number", pageNumber.ToString());
        Response.Headers.Add("X-Page-Size", pageSize.ToString());

        return Ok(result.Items);
    }

    // GET: api/products/5
    [HttpGet("{id}")]
    public async Task<ActionResult<ProductDTO>> GetProduct(int id)
    {
        var product = await _productService.GetProductByIdAsync(id);

        if (product == null)
            return NotFound(new { message = $"Product with ID {id} not found" });

        return Ok(product);
    }

    // POST: api/products
    [HttpPost]
    [Authorize(Roles = "Admin")]
    public async Task<ActionResult<ProductDTO>> CreateProduct(CreateProductDTO dto)
    {
        if (!ModelState.IsValid)
            return BadRequest(ModelState);

        var product = await _productService.CreateProductAsync(dto);

        return CreatedAtAction(nameof(GetProduct), new { id = product.ProductID }, product);
    }

    // PUT: api/products/5
    [HttpPut("{id}")]
    [Authorize(Roles = "Admin")]
    public async Task<IActionResult> UpdateProduct(int id, UpdateProductDTO dto)
    {
        if (id != dto.ProductID)
            return BadRequest(new { message = "ID mismatch" });

        var result = await _productService.UpdateProductAsync(dto);

        if (!result)
            return NotFound(new { message = $"Product with ID {id} not found" });

        return NoContent();
    }

    // DELETE: api/products/5
    [HttpDelete("{id}")]
    [Authorize(Roles = "Admin")]
    public async Task<IActionResult> DeleteProduct(int id)
    {
        var result = await _productService.DeleteProductAsync(id);

        if (!result)
            return NotFound(new { message = $"Product with ID {id} not found" });

        return NoContent();
    }

    // GET: api/products/category/5
    [HttpGet("category/{categoryId}")]
    public async Task<ActionResult<IEnumerable<ProductDTO>>> GetProductsByCategory(int categoryId)
    {
        var products = await _productService.GetProductsByCategoryAsync(categoryId);
        return Ok(products);
    }

    // GET: api/products/search?keyword=chai
    [HttpGet("search")]
    public async Task<ActionResult<IEnumerable<ProductDTO>>> SearchProducts([FromQuery] string keyword)
    {
        var products = await _productService.SearchProductsAsync(keyword);
        return Ok(products);
    }
}
```

**OrdersController.cs**
```csharp
[ApiController]
[Route("api/[controller]")]
[Authorize]
public class OrdersController : ControllerBase
{
    private readonly IOrderService _orderService;

    // GET: api/orders
    [HttpGet]
    public async Task<ActionResult<IEnumerable<OrderDTO>>> GetOrders(
        [FromQuery] DateTime? fromDate = null,
        [FromQuery] DateTime? toDate = null,
        [FromQuery] string customerId = null)
    {
        var orders = await _orderService.GetOrdersAsync(fromDate, toDate, customerId);
        return Ok(orders);
    }

    // GET: api/orders/5
    [HttpGet("{id}")]
    public async Task<ActionResult<OrderDTO>> GetOrder(int id)
    {
        var order = await _orderService.GetOrderByIdAsync(id);

        if (order == null)
            return NotFound();

        return Ok(order);
    }

    // POST: api/orders
    [HttpPost]
    public async Task<ActionResult<OrderDTO>> CreateOrder(CreateOrderDTO dto)
    {
        var order = await _orderService.CreateOrderAsync(dto);
        return CreatedAtAction(nameof(GetOrder), new { id = order.OrderID }, order);
    }

    // GET: api/orders/customer/ALFKI
    [HttpGet("customer/{customerId}")]
    public async Task<ActionResult<IEnumerable<OrderDTO>>> GetOrdersByCustomer(string customerId)
    {
        var orders = await _orderService.GetOrdersByCustomerAsync(customerId);
        return Ok(orders);
    }

    // GET: api/orders/statistics
    [HttpGet("statistics")]
    public async Task<ActionResult<OrderStatisticsDTO>> GetStatistics(
        [FromQuery] DateTime fromDate,
        [FromQuery] DateTime toDate)
    {
        var stats = await _orderService.GetOrderStatisticsAsync(fromDate, toDate);
        return Ok(stats);
    }
}
```

#### 3. Authentication & Authorization với JWT

**AuthController.cs**
```csharp
[ApiController]
[Route("api/[controller]")]
public class AuthController : ControllerBase
{
    private readonly IConfiguration _configuration;
    private readonly IUserService _userService;

    // POST: api/auth/login
    [HttpPost("login")]
    public async Task<IActionResult> Login([FromBody] LoginDTO dto)
    {
        var user = await _userService.ValidateUserAsync(dto.Username, dto.Password);

        if (user == null)
            return Unauthorized(new { message = "Invalid username or password" });

        var token = GenerateJwtToken(user);

        return Ok(new
        {
            token = token,
            expiration = DateTime.UtcNow.AddMinutes(60),
            user = new
            {
                id = user.Id,
                username = user.Username,
                role = user.Role
            }
        });
    }

    // POST: api/auth/register
    [HttpPost("register")]
    public async Task<IActionResult> Register([FromBody] RegisterDTO dto)
    {
        var user = await _userService.CreateUserAsync(dto);

        if (user == null)
            return BadRequest(new { message = "Username already exists" });

        return Ok(new { message = "User created successfully" });
    }

    private string GenerateJwtToken(User user)
    {
        var securityKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(_configuration["Jwt:Key"]));
        var credentials = new SigningCredentials(securityKey, SecurityAlgorithms.HmacSha256);

        var claims = new[]
        {
            new Claim(ClaimTypes.NameIdentifier, user.Id.ToString()),
            new Claim(ClaimTypes.Name, user.Username),
            new Claim(ClaimTypes.Role, user.Role),
            new Claim(JwtRegisteredClaimNames.Jti, Guid.NewGuid().ToString())
        };

        var token = new JwtSecurityToken(
            issuer: _configuration["Jwt:Issuer"],
            audience: _configuration["Jwt:Audience"],
            claims: claims,
            expires: DateTime.UtcNow.AddMinutes(Convert.ToDouble(_configuration["Jwt:ExpireMinutes"])),
            signingCredentials: credentials
        );

        return new JwtSecurityTokenHandler().WriteToken(token);
    }
}
```

**Cấu hình JWT trong Program.cs**
```csharp
// Add JWT Authentication
builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(options =>
    {
        options.TokenValidationParameters = new TokenValidationParameters
        {
            ValidateIssuer = true,
            ValidateAudience = true,
            ValidateLifetime = true,
            ValidateIssuerSigningKey = true,
            ValidIssuer = builder.Configuration["Jwt:Issuer"],
            ValidAudience = builder.Configuration["Jwt:Audience"],
            IssuerSigningKey = new SymmetricSecurityKey(
                Encoding.UTF8.GetBytes(builder.Configuration["Jwt:Key"]))
        };
    });

builder.Services.AddAuthorization();
```

#### 4. Middleware & Error Handling

**ErrorHandlingMiddleware.cs**
```csharp
public class ErrorHandlingMiddleware
{
    private readonly RequestDelegate _next;
    private readonly ILogger<ErrorHandlingMiddleware> _logger;

    public ErrorHandlingMiddleware(RequestDelegate next, ILogger<ErrorHandlingMiddleware> logger)
    {
        _next = next;
        _logger = logger;
    }

    public async Task InvokeAsync(HttpContext context)
    {
        try
        {
            await _next(context);
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "An unhandled exception occurred");
            await HandleExceptionAsync(context, ex);
        }
    }

    private static Task HandleExceptionAsync(HttpContext context, Exception exception)
    {
        var statusCode = exception switch
        {
            ArgumentNullException => StatusCodes.Status400BadRequest,
            KeyNotFoundException => StatusCodes.Status404NotFound,
            UnauthorizedAccessException => StatusCodes.Status401Unauthorized,
            _ => StatusCodes.Status500InternalServerError
        };

        context.Response.ContentType = "application/json";
        context.Response.StatusCode = statusCode;

        var response = new
        {
            statusCode = statusCode,
            message = exception.Message,
            details = exception.StackTrace
        };

        return context.Response.WriteAsJsonAsync(response);
    }
}
```

#### 5. Swagger Documentation

**Cấu hình Swagger trong Program.cs**
```csharp
builder.Services.AddSwaggerGen(c =>
{
    c.SwaggerDoc("v1", new OpenApiInfo
    {
        Title = "Northwind API",
        Version = "v1",
        Description = "API for Northwind Database Management",
        Contact = new OpenApiContact
        {
            Name = "Your Name",
            Email = "your.email@example.com"
        }
    });

    // Add JWT Authentication to Swagger
    c.AddSecurityDefinition("Bearer", new OpenApiSecurityScheme
    {
        Description = "JWT Authorization header using the Bearer scheme. Enter 'Bearer' [space] and then your token",
        Name = "Authorization",
        In = ParameterLocation.Header,
        Type = SecuritySchemeType.ApiKey,
        Scheme = "Bearer"
    });

    c.AddSecurityRequirement(new OpenApiSecurityRequirement
    {
        {
            new OpenApiSecurityScheme
            {
                Reference = new OpenApiReference
                {
                    Type = ReferenceType.SecurityScheme,
                    Id = "Bearer"
                }
            },
            Array.Empty<string>()
        }
    });
});
```

#### 6. CORS Configuration

```csharp
builder.Services.AddCors(options =>
{
    options.AddPolicy("AllowAll", builder =>
    {
        builder.AllowAnyOrigin()
               .AllowAnyMethod()
               .AllowAnyHeader();
    });
});

// Trong pipeline
app.UseCors("AllowAll");
```

#### 7. Filtering, Sorting, Pagination

**QueryParameters.cs**
```csharp
public class ProductQueryParameters
{
    private const int MaxPageSize = 100;
    private int _pageSize = 10;

    public int PageNumber { get; set; } = 1;

    public int PageSize
    {
        get => _pageSize;
        set => _pageSize = (value > MaxPageSize) ? MaxPageSize : value;
    }

    public string SearchTerm { get; set; }
    public string SortBy { get; set; } = "ProductName";
    public bool SortDescending { get; set; } = false;
    public int? CategoryId { get; set; }
    public decimal? MinPrice { get; set; }
    public decimal? MaxPrice { get; set; }
}
```

**ProductService with advanced filtering**
```csharp
public async Task<PagedResult<ProductDTO>> GetProductsAsync(ProductQueryParameters parameters)
{
    var query = _context.Products
        .Include(p => p.Category)
        .Include(p => p.Supplier)
        .AsQueryable();

    // Filtering
    if (!string.IsNullOrWhiteSpace(parameters.SearchTerm))
    {
        query = query.Where(p => p.ProductName.Contains(parameters.SearchTerm));
    }

    if (parameters.CategoryId.HasValue)
    {
        query = query.Where(p => p.CategoryID == parameters.CategoryId);
    }

    if (parameters.MinPrice.HasValue)
    {
        query = query.Where(p => p.UnitPrice >= parameters.MinPrice);
    }

    if (parameters.MaxPrice.HasValue)
    {
        query = query.Where(p => p.UnitPrice <= parameters.MaxPrice);
    }

    // Sorting
    query = parameters.SortBy.ToLower() switch
    {
        "price" => parameters.SortDescending
            ? query.OrderByDescending(p => p.UnitPrice)
            : query.OrderBy(p => p.UnitPrice),
        "stock" => parameters.SortDescending
            ? query.OrderByDescending(p => p.UnitsInStock)
            : query.OrderBy(p => p.UnitsInStock),
        _ => parameters.SortDescending
            ? query.OrderByDescending(p => p.ProductName)
            : query.OrderBy(p => p.ProductName)
    };

    // Pagination
    var totalCount = await query.CountAsync();
    var items = await query
        .Skip((parameters.PageNumber - 1) * parameters.PageSize)
        .Take(parameters.PageSize)
        .Select(p => new ProductDTO
        {
            ProductID = p.ProductID,
            ProductName = p.ProductName,
            CategoryName = p.Category.CategoryName,
            SupplierName = p.Supplier.CompanyName,
            UnitPrice = p.UnitPrice,
            UnitsInStock = p.UnitsInStock
        })
        .ToListAsync();

    return new PagedResult<ProductDTO>
    {
        Items = items,
        TotalCount = totalCount,
        PageNumber = parameters.PageNumber,
        PageSize = parameters.PageSize
    };
}
```

#### 8. Testing API với Postman/Swagger

**Các endpoint cần test:**
```
GET    /api/products
GET    /api/products/5
POST   /api/products
PUT    /api/products/5
DELETE /api/products/5
GET    /api/products/category/1
GET    /api/products?pageNumber=1&pageSize=10&search=chai

GET    /api/orders
GET    /api/orders/5
POST   /api/orders
GET    /api/orders/customer/ALFKI
GET    /api/orders/statistics?fromDate=2024-01-01&toDate=2024-12-31

POST   /api/auth/login
POST   /api/auth/register
```

#### 9. Deploy lên Azure/IIS

**a. Deploy lên Azure App Service**
```bash
# Publish từ Visual Studio 2022
1. Right-click project → Publish
2. Chọn Azure → Azure App Service (Windows)
3. Tạo resource group mới
4. Configure connection string
5. Publish

# Deploy bằng Azure CLI với .NET 10
az webapp up --name northwind-api --resource-group myResourceGroup --runtime "DOTNET|10.0" --os-type Windows

# Hoặc deploy lên Linux
az webapp up --name northwind-api --resource-group myResourceGroup --runtime "DOTNET|10.0" --os-type Linux

# Kiểm tra runtime có sẵn
az webapp list-runtimes --os-type windows
```

**b. Deploy lên IIS**
```bash
# Publish với .NET 10
dotnet publish -c Release -o ./publish -r win-x64 --self-contained false

# Copy thư mục publish lên server
# Cài đặt .NET 10 Runtime trên server (nếu dùng framework-dependent)
# Tạo Application Pool trong IIS (No Managed Code)
# Add Website và trỏ đến thư mục publish
# Configure phải chọn .NET CLR version: No Managed Code
```

#### 10. Chức năng cần hoàn thiện

- ✅ RESTful API đầy đủ cho Products, Orders, Customers
- ✅ JWT Authentication & Authorization
- ✅ Swagger documentation
- ✅ Error handling middleware
- ✅ CORS configuration
- ✅ Filtering, Sorting, Pagination
- ✅ Unit tests với xUnit
- ✅ Deploy lên Azure hoặc IIS

#### 11. Kiểm thử

- Test tất cả endpoints với Postman
- Test authentication flow
- Test authorization (admin vs user)
- Test pagination với dữ liệu lớn
- Test error handling
- Load testing với JMeter

---

## KẾ HOẠCH TUẦN 7

### Mục tiêu
- Xây dựng **ASP.NET Core MVC Web Application**
- Tích hợp với **Web API** đã tạo ở tuần 6
- Áp dụng **Razor Pages** và **ViewComponents**
- Implement **SignalR** cho real-time updates
- Deploy toàn bộ hệ thống

### Nội dung thực hiện

#### 1. Tạo ASP.NET Core MVC Project

**a. Cấu trúc project**
```
NorthwindMVC/
├── Controllers/
│   ├── HomeController.cs
│   ├── ProductsController.cs
│   ├── OrdersController.cs
│   └── AccountController.cs
├── Views/
│   ├── Shared/
│   │   ├── _Layout.cshtml
│   │   └── _LoginPartial.cshtml
│   ├── Home/
│   │   └── Index.cshtml
│   ├── Products/
│   │   ├── Index.cshtml
│   │   ├── Create.cshtml
│   │   └── Edit.cshtml
│   └── Orders/
│       └── Index.cshtml
├── ViewComponents/
│   └── ProductSummaryViewComponent.cs
├── Services/
│   └── ApiService.cs
├── wwwroot/
│   ├── css/
│   ├── js/
│   └── lib/
└── Program.cs
```

**b. Cấu hình appsettings.json**
```json
{
  "ApiSettings": {
    "BaseUrl": "https://localhost:7001/api",
    "Timeout": 30
  },
  "ConnectionStrings": {
    "NorthwindConnection": "Server=.;Database=Northwind;Trusted_Connection=True;"
  }
}
```

#### 2. Tích hợp với Web API

**ApiService.cs - HTTP Client Service**
```csharp
public class ApiService
{
    private readonly HttpClient _httpClient;
    private readonly IConfiguration _configuration;
    private readonly IHttpContextAccessor _httpContextAccessor;

    public ApiService(HttpClient httpClient, IConfiguration configuration, IHttpContextAccessor httpContextAccessor)
    {
        _httpClient = httpClient;
        _configuration = configuration;
        _httpContextAccessor = httpContextAccessor;

        _httpClient.BaseAddress = new Uri(_configuration["ApiSettings:BaseUrl"]);
        _httpClient.Timeout = TimeSpan.FromSeconds(Convert.ToDouble(_configuration["ApiSettings:Timeout"]));
    }

    private void SetAuthorizationHeader()
    {
        var token = _httpContextAccessor.HttpContext?.Session.GetString("JwtToken");
        if (!string.IsNullOrEmpty(token))
        {
            _httpClient.DefaultRequestHeaders.Authorization =
                new AuthenticationHeaderValue("Bearer", token);
        }
    }

    // Products
    public async Task<List<ProductDTO>> GetProductsAsync(int pageNumber = 1, int pageSize = 10, string search = null)
    {
        SetAuthorizationHeader();
        var response = await _httpClient.GetAsync($"products?pageNumber={pageNumber}&pageSize={pageSize}&search={search}");
        response.EnsureSuccessStatusCode();
        return await response.Content.ReadFromJsonAsync<List<ProductDTO>>();
    }

    public async Task<ProductDTO> GetProductByIdAsync(int id)
    {
        SetAuthorizationHeader();
        var response = await _httpClient.GetAsync($"products/{id}");
        response.EnsureSuccessStatusCode();
        return await response.Content.ReadFromJsonAsync<ProductDTO>();
    }

    public async Task<ProductDTO> CreateProductAsync(CreateProductDTO dto)
    {
        SetAuthorizationHeader();
        var response = await _httpClient.PostAsJsonAsync("products", dto);
        response.EnsureSuccessStatusCode();
        return await response.Content.ReadFromJsonAsync<ProductDTO>();
    }

    public async Task UpdateProductAsync(int id, UpdateProductDTO dto)
    {
        SetAuthorizationHeader();
        var response = await _httpClient.PutAsJsonAsync($"products/{id}", dto);
        response.EnsureSuccessStatusCode();
    }

    public async Task DeleteProductAsync(int id)
    {
        SetAuthorizationHeader();
        var response = await _httpClient.DeleteAsync($"products/{id}");
        response.EnsureSuccessStatusCode();
    }

    // Authentication
    public async Task<LoginResultDTO> LoginAsync(LoginDTO dto)
    {
        var response = await _httpClient.PostAsJsonAsync("auth/login", dto);
        response.EnsureSuccessStatusCode();
        return await response.Content.ReadFromJsonAsync<LoginResultDTO>();
    }
}
```

**Đăng ký trong Program.cs**
```csharp
builder.Services.AddHttpClient<ApiService>();
builder.Services.AddHttpContextAccessor();
builder.Services.AddSession(options =>
{
    options.IdleTimeout = TimeSpan.FromMinutes(30);
    options.Cookie.HttpOnly = true;
    options.Cookie.IsEssential = true;
});
```

#### 3. Controllers & Views

**ProductsController.cs**
```csharp
public class ProductsController : Controller
{
    private readonly ApiService _apiService;

    public ProductsController(ApiService apiService)
    {
        _apiService = apiService;
    }

    // GET: Products
    public async Task<IActionResult> Index(int pageNumber = 1, string search = null)
    {
        try
        {
            var products = await _apiService.GetProductsAsync(pageNumber, 10, search);
            ViewBag.PageNumber = pageNumber;
            ViewBag.Search = search;
            return View(products);
        }
        catch (HttpRequestException ex)
        {
            TempData["Error"] = "Failed to load products: " + ex.Message;
            return View(new List<ProductDTO>());
        }
    }

    // GET: Products/Create
    public async Task<IActionResult> Create()
    {
        ViewBag.Categories = await _apiService.GetCategoriesAsync();
        ViewBag.Suppliers = await _apiService.GetSuppliersAsync();
        return View();
    }

    // POST: Products/Create
    [HttpPost]
    [ValidateAntiForgeryToken]
    public async Task<IActionResult> Create(CreateProductDTO dto)
    {
        if (!ModelState.IsValid)
        {
            ViewBag.Categories = await _apiService.GetCategoriesAsync();
            ViewBag.Suppliers = await _apiService.GetSuppliersAsync();
            return View(dto);
        }

        try
        {
            await _apiService.CreateProductAsync(dto);
            TempData["Success"] = "Product created successfully!";
            return RedirectToAction(nameof(Index));
        }
        catch (HttpRequestException ex)
        {
            ModelState.AddModelError("", "Failed to create product: " + ex.Message);
            ViewBag.Categories = await _apiService.GetCategoriesAsync();
            ViewBag.Suppliers = await _apiService.GetSuppliersAsync();
            return View(dto);
        }
    }

    // GET: Products/Edit/5
    public async Task<IActionResult> Edit(int id)
    {
        var product = await _apiService.GetProductByIdAsync(id);

        if (product == null)
            return NotFound();

        ViewBag.Categories = await _apiService.GetCategoriesAsync();
        ViewBag.Suppliers = await _apiService.GetSuppliersAsync();

        return View(product);
    }

    // POST: Products/Edit/5
    [HttpPost]
    [ValidateAntiForgeryToken]
    public async Task<IActionResult> Edit(int id, UpdateProductDTO dto)
    {
        if (id != dto.ProductID)
            return BadRequest();

        if (!ModelState.IsValid)
        {
            ViewBag.Categories = await _apiService.GetCategoriesAsync();
            ViewBag.Suppliers = await _apiService.GetSuppliersAsync();
            return View(dto);
        }

        try
        {
            await _apiService.UpdateProductAsync(id, dto);
            TempData["Success"] = "Product updated successfully!";
            return RedirectToAction(nameof(Index));
        }
        catch (HttpRequestException ex)
        {
            ModelState.AddModelError("", ex.Message);
            ViewBag.Categories = await _apiService.GetCategoriesAsync();
            ViewBag.Suppliers = await _apiService.GetSuppliersAsync();
            return View(dto);
        }
    }

    // POST: Products/Delete/5
    [HttpPost]
    [ValidateAntiForgeryToken]
    public async Task<IActionResult> Delete(int id)
    {
        try
        {
            await _apiService.DeleteProductAsync(id);
            TempData["Success"] = "Product deleted successfully!";
        }
        catch (HttpRequestException ex)
        {
            TempData["Error"] = "Failed to delete product: " + ex.Message;
        }

        return RedirectToAction(nameof(Index));
    }
}
```

**Views/Products/Index.cshtml**
```html
@model List<ProductDTO>

@{
    ViewData["Title"] = "Products";
}

<div class="container mt-4">
    <div class="d-flex justify-content-between align-items-center mb-3">
        <h2>Products Management</h2>
        <a asp-action="Create" class="btn btn-primary">
            <i class="bi bi-plus-circle"></i> Add New Product
        </a>
    </div>

    @if (TempData["Success"] != null)
    {
        <div class="alert alert-success alert-dismissible fade show" role="alert">
            @TempData["Success"]
            <button type="button" class="btn-close" data-bs-dismiss="alert"></button>
        </div>
    }

    <div class="card">
        <div class="card-body">
            <form asp-action="Index" method="get" class="mb-3">
                <div class="input-group">
                    <input type="text" name="search" class="form-control" placeholder="Search products..." value="@ViewBag.Search">
                    <button class="btn btn-outline-secondary" type="submit">Search</button>
                </div>
            </form>

            <table class="table table-striped table-hover">
                <thead>
                    <tr>
                        <th>ID</th>
                        <th>Product Name</th>
                        <th>Category</th>
                        <th>Supplier</th>
                        <th>Price</th>
                        <th>Stock</th>
                        <th>Actions</th>
                    </tr>
                </thead>
                <tbody>
                    @foreach (var product in Model)
                    {
                        <tr>
                            <td>@product.ProductID</td>
                            <td>@product.ProductName</td>
                            <td>@product.CategoryName</td>
                            <td>@product.SupplierName</td>
                            <td>@product.UnitPrice?.ToString("C")</td>
                            <td>
                                <span class="badge bg-@(product.UnitsInStock > 20 ? "success" : product.UnitsInStock > 0 ? "warning" : "danger")">
                                    @product.UnitsInStock
                                </span>
                            </td>
                            <td>
                                <a asp-action="Edit" asp-route-id="@product.ProductID" class="btn btn-sm btn-warning">Edit</a>
                                <form asp-action="Delete" asp-route-id="@product.ProductID" method="post" style="display:inline;">
                                    @Html.AntiForgeryToken()
                                    <button type="submit" class="btn btn-sm btn-danger" onclick="return confirm('Are you sure?')">Delete</button>
                                </form>
                            </td>
                        </tr>
                    }
                </tbody>
            </table>

            <nav>
                <ul class="pagination">
                    <li class="page-item @(ViewBag.PageNumber == 1 ? "disabled" : "")">
                        <a class="page-link" asp-action="Index" asp-route-pageNumber="@(ViewBag.PageNumber - 1)" asp-route-search="@ViewBag.Search">Previous</a>
                    </li>
                    <li class="page-item active">
                        <span class="page-link">@ViewBag.PageNumber</span>
                    </li>
                    <li class="page-item">
                        <a class="page-link" asp-action="Index" asp-route-pageNumber="@(ViewBag.PageNumber + 1)" asp-route-search="@ViewBag.Search">Next</a>
                    </li>
                </ul>
            </nav>
        </div>
    </div>
</div>
```

#### 4. Authentication & Authorization

**AccountController.cs**
```csharp
public class AccountController : Controller
{
    private readonly ApiService _apiService;

    public AccountController(ApiService apiService)
    {
        _apiService = apiService;
    }

    [HttpGet]
    public IActionResult Login(string returnUrl = null)
    {
        ViewData["ReturnUrl"] = returnUrl;
        return View();
    }

    [HttpPost]
    [ValidateAntiForgeryToken]
    public async Task<IActionResult> Login(LoginDTO dto, string returnUrl = null)
    {
        if (!ModelState.IsValid)
            return View(dto);

        try
        {
            var result = await _apiService.LoginAsync(dto);

            // Lưu JWT token vào session
            HttpContext.Session.SetString("JwtToken", result.Token);
            HttpContext.Session.SetString("Username", result.User.Username);
            HttpContext.Session.SetString("Role", result.User.Role);

            TempData["Success"] = $"Welcome back, {result.User.Username}!";

            if (!string.IsNullOrEmpty(returnUrl) && Url.IsLocalUrl(returnUrl))
                return Redirect(returnUrl);

            return RedirectToAction("Index", "Home");
        }
        catch (HttpRequestException)
        {
            ModelState.AddModelError("", "Invalid username or password");
            return View(dto);
        }
    }

    [HttpPost]
    [ValidateAntiForgeryToken]
    public IActionResult Logout()
    {
        HttpContext.Session.Clear();
        TempData["Success"] = "You have been logged out successfully";
        return RedirectToAction("Login");
    }
}
```

**Authorization Filter**
```csharp
public class AuthorizeAttribute : ActionFilterAttribute
{
    public override void OnActionExecuting(ActionExecutingContext context)
    {
        var token = context.HttpContext.Session.GetString("JwtToken");

        if (string.IsNullOrEmpty(token))
        {
            context.Result = new RedirectToActionResult("Login", "Account",
                new { returnUrl = context.HttpContext.Request.Path });
        }
    }
}
```

#### 5. View Components

**ProductSummaryViewComponent.cs**
```csharp
public class ProductSummaryViewComponent : ViewComponent
{
    private readonly ApiService _apiService;

    public ProductSummaryViewComponent(ApiService apiService)
    {
        _apiService = apiService;
    }

    public async Task<IViewComponentResult> InvokeAsync()
    {
        var statistics = await _apiService.GetProductStatisticsAsync();
        return View(statistics);
    }
}
```

**Views/Shared/Components/ProductSummary/Default.cshtml**
```html
@model ProductStatisticsDTO

<div class="row">
    <div class="col-md-3">
        <div class="card text-white bg-primary mb-3">
            <div class="card-body">
                <h5 class="card-title">Total Products</h5>
                <h2>@Model.TotalProducts</h2>
            </div>
        </div>
    </div>
    <div class="col-md-3">
        <div class="card text-white bg-success mb-3">
            <div class="card-body">
                <h5 class="card-title">In Stock</h5>
                <h2>@Model.InStockProducts</h2>
            </div>
        </div>
    </div>
    <div class="col-md-3">
        <div class="card text-white bg-warning mb-3">
            <div class="card-body">
                <h5 class="card-title">Low Stock</h5>
                <h2>@Model.LowStockProducts</h2>
            </div>
        </div>
    </div>
    <div class="col-md-3">
        <div class="card text-white bg-danger mb-3">
            <div class="card-body">
                <h5 class="card-title">Out of Stock</h5>
                <h2>@Model.OutOfStockProducts</h2>
            </div>
        </div>
    </div>
</div>
```

#### 6. SignalR cho Real-time Updates

**Cài đặt SignalR cho .NET 10**
```bash
# SignalR đã được tích hợp sẵn trong ASP.NET Core 10
# Không cần cài thêm package riêng, chỉ cần sử dụng

# Nếu cần client library:
dotnet add package Microsoft.AspNetCore.SignalR.Client --version 10.0.0
```

**ProductHub.cs**
```csharp
public class ProductHub : Hub
{
    public async Task NotifyProductUpdate(string message)
    {
        await Clients.All.SendAsync("ReceiveProductUpdate", message);
    }

    public async Task NotifyNewOrder(int orderId, string customerName)
    {
        await Clients.All.SendAsync("ReceiveNewOrder", orderId, customerName);
    }
}
```

**Cấu hình trong Program.cs**
```csharp
builder.Services.AddSignalR();

app.MapHub<ProductHub>("/productHub");
```

**JavaScript client (wwwroot/js/signalr-client.js)**
```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/productHub")
    .build();

connection.on("ReceiveProductUpdate", function (message) {
    toastr.info(message);
    // Refresh product list
    location.reload();
});

connection.on("ReceiveNewOrder", function (orderId, customerName) {
    toastr.success(`New order #${orderId} from ${customerName}`);
});

connection.start().catch(function (err) {
    console.error(err.toString());
});
```

#### 7. Charts với Chart.js

**Views/Home/Index.cshtml - Dashboard**
```html
@{
    ViewData["Title"] = "Dashboard";
}

<div class="container mt-4">
    <h2>Dashboard</h2>

    @await Component.InvokeAsync("ProductSummary")

    <div class="row mt-4">
        <div class="col-md-6">
            <div class="card">
                <div class="card-header">
                    <h5>Revenue by Month</h5>
                </div>
                <div class="card-body">
                    <canvas id="revenueChart"></canvas>
                </div>
            </div>
        </div>
        <div class="col-md-6">
            <div class="card">
                <div class="card-header">
                    <h5>Top Products</h5>
                </div>
                <div class="card-body">
                    <canvas id="productsChart"></canvas>
                </div>
            </div>
        </div>
    </div>
</div>

@section Scripts {
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script>
        // Revenue Chart
        fetch('/api/statistics/revenue-by-month')
            .then(response => response.json())
            .then(data => {
                const ctx = document.getElementById('revenueChart').getContext('2d');
                new Chart(ctx, {
                    type: 'line',
                    data: {
                        labels: data.map(d => d.month),
                        datasets: [{
                            label: 'Revenue',
                            data: data.map(d => d.revenue),
                            borderColor: 'rgb(75, 192, 192)',
                            tension: 0.1
                        }]
                    }
                });
            });

        // Top Products Chart
        fetch('/api/statistics/top-products')
            .then(response => response.json())
            .then(data => {
                const ctx = document.getElementById('productsChart').getContext('2d');
                new Chart(ctx, {
                    type: 'bar',
                    data: {
                        labels: data.map(d => d.productName),
                        datasets: [{
                            label: 'Sales',
                            data: data.map(d => d.quantity),
                            backgroundColor: 'rgba(54, 162, 235, 0.5)'
                        }]
                    }
                });
            });
    </script>
}
```

#### 8. Responsive Design với Bootstrap 5

**Views/Shared/_Layout.cshtml**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>@ViewData["Title"] - Northwind Management</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.10.0/font/bootstrap-icons.css">
    <link href="https://cdnjs.cloudflare.com/ajax/libs/toastr.js/latest/toastr.min.css" rel="stylesheet" />
</head>
<body>
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
        <div class="container-fluid">
            <a class="navbar-brand" href="/">Northwind</a>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarNav">
                <ul class="navbar-nav me-auto">
                    <li class="nav-item">
                        <a class="nav-link" asp-controller="Home" asp-action="Index">Dashboard</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" asp-controller="Products" asp-action="Index">Products</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" asp-controller="Orders" asp-action="Index">Orders</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" asp-controller="Customers" asp-action="Index">Customers</a>
                    </li>
                </ul>
                <ul class="navbar-nav">
                    @if (Context.Session.GetString("Username") != null)
                    {
                        <li class="nav-item dropdown">
                            <a class="nav-link dropdown-toggle" href="#" id="userDropdown" data-bs-toggle="dropdown">
                                <i class="bi bi-person-circle"></i> @Context.Session.GetString("Username")
                            </a>
                            <ul class="dropdown-menu dropdown-menu-end">
                                <li><a class="dropdown-item" href="#">Profile</a></li>
                                <li><hr class="dropdown-divider"></li>
                                <li>
                                    <form asp-controller="Account" asp-action="Logout" method="post">
                                        @Html.AntiForgeryToken()
                                        <button type="submit" class="dropdown-item">Logout</button>
                                    </form>
                                </li>
                            </ul>
                        </li>
                    }
                    else
                    {
                        <li class="nav-item">
                            <a class="nav-link" asp-controller="Account" asp-action="Login">Login</a>
                        </li>
                    }
                </ul>
            </div>
        </div>
    </nav>

    <main class="container">
        @RenderBody()
    </main>

    <footer class="footer mt-auto py-3 bg-light">
        <div class="container text-center">
            <span class="text-muted">&copy; 2025 Northwind Management System</span>
        </div>
    </footer>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/toastr.js/latest/toastr.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/microsoft-signalr/6.0.1/signalr.min.js"></script>
    <script src="~/js/signalr-client.js"></script>
    @await RenderSectionAsync("Scripts", required: false)
</body>
</html>
```

#### 9. Deployment với .NET 10

**a. Publish MVC Web App**
```bash
# Framework-dependent deployment (.NET 10 Runtime cần có trên server)
dotnet publish -c Release -o ./publish

# Self-contained deployment (bao gồm cả runtime)
dotnet publish -c Release -o ./publish -r win-x64 --self-contained true

# AOT (Ahead-of-Time) compilation - Tính năng mới của .NET 10
dotnet publish -c Release -o ./publish -r win-x64 -p:PublishAot=true
```

**b. Deploy cả Web API và MVC lên Azure**
```bash
# Web API với .NET 10
az webapp up --name northwind-api --resource-group northwind-rg --runtime "DOTNET|10.0"

# MVC Web App với .NET 10
az webapp up --name northwind-web --resource-group northwind-rg --runtime "DOTNET|10.0"

# Deploy với Docker container (khuyến nghị cho production)
az acr build --registry myregistry --image northwind-api:latest .
az webapp create --resource-group northwind-rg --plan myAppServicePlan --name northwind-api --deployment-container-image-name myregistry.azurecr.io/northwind-api:latest
```

**c. Cấu hình HTTPS và SSL**
- Cấu hình SSL certificate trong Azure (Let's Encrypt hoặc Custom)
- Force HTTPS redirect trong Program.cs
- Update CORS cho production
- Enable HTTP/3 (mới trong .NET 10)

#### 10. Chức năng cần hoàn thiện

- ✅ ASP.NET Core MVC Web Application đầy đủ
- ✅ Tích hợp với Web API
- ✅ Authentication với JWT
- ✅ View Components cho UI reusable
- ✅ SignalR cho real-time updates
- ✅ Responsive design với Bootstrap 5
- ✅ Charts và Dashboard
- ✅ Deploy lên Azure

#### 11. Kiểm thử tổng thể

- Test toàn bộ luồng: WinForm → API → MVC
- Test authentication trên cả 3 platforms
- Test real-time updates với SignalR
- Test responsive design trên mobile/tablet
- Performance testing toàn hệ thống
- Security testing (OWASP Top 10)

---

## TỔNG KẾT VÀ ĐÁNH GIÁ

### Checklist hoàn thành dự án

**Tuần 1:**
- [ ] Database Northwind đã được cài đặt
- [ ] Các lớp DAL cho Products, Categories, Suppliers
- [ ] CRUD đầy đủ cho Sản phẩm
- [ ] Giao diện quản lý Sản phẩm
- [ ] Validation và xử lý lỗi

**Tuần 2:**
- [ ] Các lớp DAL cho Orders, Customers, Employees
- [ ] CRUD đầy đủ cho Đơn hàng, Khách hàng, Nhân viên
- [ ] Chức năng tìm kiếm nâng cao
- [ ] Cập nhật ComboBox động
- [ ] Tính toán tổng tiền đơn hàng

**Tuần 3:**
- [ ] Module thống kê đầy đủ với biểu đồ
- [ ] Xuất Excel cho tất cả modules
- [ ] In danh sách và hóa đơn
- [ ] Dashboard tổng quan
- [ ] Form cài đặt

**Tuần 4:**
- [ ] Cài đặt Entity Framework Core
- [ ] Generate entities từ database (Database First)
- [ ] Tạo tất cả Repositories và DTOs
- [ ] Refactor toàn bộ WinForm để sử dụng EF
- [ ] Testing performance ADO.NET vs EF

**Tuần 5:**
- [ ] Áp dụng LINQ queries toàn diện
- [ ] Tạo Stored Procedures
- [ ] Implement Repository Pattern và Unit of Work
- [ ] Xuất/Nhập dữ liệu XML
- [ ] Tối ưu hóa performance

**Tuần 6:**
- [ ] Xây dựng ASP.NET Core Web API
- [ ] RESTful API với JWT Authentication
- [ ] Swagger documentation
- [ ] Filtering, Sorting, Pagination
- [ ] Deploy lên Azure/IIS

**Tuần 7:**
- [ ] Xây dựng ASP.NET Core MVC Web App
- [ ] Tích hợp với Web API
- [ ] SignalR real-time updates
- [ ] Dashboard với Chart.js
- [ ] Deploy toàn bộ hệ thống

### Công nghệ và thư viện sử dụng

**PHẦN 1: CƠ BẢN (Tuần 1-3)**

**Framework:**
- **.NET 10** (latest)
- **C# 13** (với .NET 10)
- WinForms trên .NET 10

**Database:**
- SQL Server 2019 trở lên (khuyến nghị 2022)
- ADO.NET (Connected & Disconnected Model)

**Thư viện:**
- **EPPlus 7.x** hoặc **ClosedXML**: Xuất Excel
- **System.Windows.Forms.DataVisualization**: Biểu đồ
- **QuestPDF** (thay thế iTextSharp): Xuất PDF hiện đại

**PHẦN 2: NÂNG CAO (Tuần 4-7)**

**Framework:**
- **.NET 10** (latest stable)
- **ASP.NET Core 10**
- **C# 13** với tính năng mới nhất

**ORM & Data Access:**
- **Entity Framework Core 10**: Database First & Code First
- **LINQ to Entities**: Truy vấn type-safe với C# 13
- **Dapper 2.1+** (tùy chọn): Micro ORM cho performance

**Web Technologies:**
- **ASP.NET Core Web API**: RESTful services
- **ASP.NET Core MVC**: Web application
- **SignalR**: Real-time communication
- **Swagger/OpenAPI**: API documentation

**Authentication & Security:**
- **JWT (JSON Web Tokens)**: Stateless authentication
- **ASP.NET Core Identity** (tùy chọn): User management
- **BCrypt.Net**: Password hashing

**Frontend:**
- **Bootstrap 5**: Responsive UI framework
- **jQuery 3.6+**: JavaScript library
- **Chart.js**: Data visualization
- **Toastr**: Notification system
- **SignalR Client**: Real-time updates

**Testing:**
- **xUnit**: Unit testing framework
- **Moq**: Mocking framework
- **Postman**: API testing
- **Selenium** (tùy chọn): UI automation testing

**DevOps & Deployment:**
- **Azure App Service**: Cloud hosting
- **IIS 10+**: On-premise hosting
- **Docker** (tùy chọn): Containerization
- **GitHub Actions** (tùy chọn): CI/CD pipeline

**Tools:**
- **Visual Studio 2022**: IDE
- **SQL Server Management Studio (SSMS)**: Database management
- **Postman**: API testing
- **Azure Data Studio** (tùy chọn): Cross-platform database tool

### Tiêu chí đánh giá

**PHẦN 1: CƠ BẢN (60 điểm)**

1. **Chức năng WinForm (30 điểm)**
   - CRUD đầy đủ cho tất cả modules (15đ)
   - Tìm kiếm và filter chính xác (8đ)
   - Thống kê, báo cáo, xuất Excel (7đ)

2. **Giao diện WinForm (10 điểm)**
   - Thân thiện, dễ sử dụng (5đ)
   - Bố cục hợp lý, validation tốt (5đ)

3. **Code Quality (15 điểm)**
   - Kiến trúc 3-layer rõ ràng (8đ)
   - Code sạch, có comment (4đ)
   - Xử lý exception đầy đủ (3đ)

4. **Database (5 điểm)**
   - Thiết kế chuẩn hóa (3đ)
   - Có ràng buộc và index (2đ)

**PHẦN 2: NÂNG CAO (40 điểm)**

5. **Entity Framework & LINQ (15 điểm)**
   - Database First implementation đúng (5đ)
   - LINQ queries hiệu quả (5đ)
   - Repository & Unit of Work pattern (5đ)

6. **Web API (15 điểm)**
   - RESTful API design chuẩn (5đ)
   - JWT Authentication hoạt động (5đ)
   - Swagger documentation đầy đủ (3đ)
   - Error handling và validation (2đ)

7. **ASP.NET Core MVC (10 điểm)**
   - Tích hợp API thành công (4đ)
   - Responsive UI với Bootstrap (3đ)
   - SignalR real-time updates (3đ)

**BONUS (10 điểm)**
- Unit testing coverage > 70% (+3đ)
- Deploy lên Azure thành công (+3đ)
- Docker containerization (+2đ)
- CI/CD pipeline (+2đ)

### Ghi chú quan trọng

⚠️ **Lưu ý khi phát triển:**
- Backup database thường xuyên
- Test kỹ trước khi chuyển sang tuần tiếp theo
- Commit code lên Git sau mỗi tính năng hoàn thành
- Viết tài liệu hướng dẫn sử dụng

💡 **Gợi ý mở rộng thêm (Tuần 8+):**

**Level 1: Tính năng nâng cao**
- **Phân quyền chi tiết**: Role-based và Claims-based authorization
- **Email notifications**: SendGrid/SMTP cho thông báo đơn hàng
- **SMS notifications**: Twilio integration
- **Payment integration**: Stripe/PayPal API
- **File upload**: Azure Blob Storage cho hình ảnh sản phẩm
- **Audit logging**: Track tất cả thay đổi dữ liệu

**Level 2: Microservices Architecture**
- Tách riêng: Product Service, Order Service, Customer Service
- API Gateway với Ocelot
- Message Queue với RabbitMQ/Azure Service Bus
- Event-driven architecture
- CQRS pattern cho read/write separation

**Level 3: Cloud-Native**
- **Containerization**: Docker Compose cho multi-container
- **Orchestration**: Kubernetes (AKS)
- **Service Mesh**: Istio/Linkerd
- **Monitoring**: Application Insights, Prometheus + Grafana
- **Distributed Tracing**: Jaeger/Zipkin
- **Cache**: Redis cho session và data caching
- **CDN**: Azure CDN cho static assets

**Level 4: Modern Frontend**
- **Blazor WebAssembly**: SPA với .NET
- **React/Angular**: Modern SPA framework
- **Mobile App**: React Native/Flutter/MAUI
- **PWA**: Progressive Web App với offline support

**Level 5: Advanced Features**
- **Machine Learning**: Product recommendations với ML.NET
- **Chatbot**: Azure Bot Service cho customer support
- **Search**: Elasticsearch/Azure Cognitive Search
- **GraphQL**: Alternative API với Hot Chocolate
- **WebHooks**: Event subscriptions cho third-party
- **Rate Limiting**: API throttling và quota management

**Level 6: Enterprise Features**
- **Multi-tenancy**: Support nhiều khách hàng
- **Internationalization (i18n)**: Multi-language support
- **Dark Mode**: Theme customization
- **Accessibility**: WCAG compliance
- **Load Testing**: K6/JMeter cho stress testing
- **Chaos Engineering**: Testing resilience

### Tính năng mới của .NET 10 & C# 13

**🆕 Những tính năng nên sử dụng trong dự án:**

**C# 13 Language Features:**
1. **Primary Constructors for all types** - Giảm boilerplate code
   ```csharp
   public class ProductService(IProductRepository repository, ILogger<ProductService> logger)
   {
       public async Task<Product> GetProduct(int id) => await repository.GetByIdAsync(id);
   }
   ```

2. **Collection Expressions** - Cú pháp ngắn gọn cho collections
   ```csharp
   int[] numbers = [1, 2, 3, 4, 5];
   List<Product> products = [product1, product2, .. otherProducts];
   ```

3. **Inline Arrays** - Performance tốt hơn
   ```csharp
   [System.Runtime.CompilerServices.InlineArray(10)]
   public struct Buffer10<T> { private T _element0; }
   ```

**.NET 10 Runtime Improvements:**
1. **Native AOT (Ahead-of-Time Compilation)** - Startup nhanh hơn, memory thấp hơn
2. **HTTP/3 by default** - Performance tốt hơn cho Web API
3. **Improved JSON Serialization** - Nhanh hơn 30% so với .NET 8
4. **Enhanced Minimal APIs** - Viết API nhanh hơn
5. **Built-in Rate Limiting** - Bảo vệ API tốt hơn

**Entity Framework Core 10:**
1. **Complex Types Support** - Map JSON columns dễ hơn
2. **Improved Performance** - Query nhanh hơn 20%
3. **Better LINQ Translation** - Hỗ trợ nhiều LINQ operators hơn
4. **JSON Columns** - Native support cho SQL Server JSON

**ASP.NET Core 10:**
1. **Keyed Services DI** - Dependency Injection linh hoạt hơn
   ```csharp
   builder.Services.AddKeyedScoped<IProductService, ProductService>("v1");
   builder.Services.AddKeyedScoped<IProductService, ProductServiceV2>("v2");
   ```

2. **Minimal APIs Improvements** - Filters và Middleware tốt hơn
3. **OpenAPI Built-in** - Không cần Swashbuckle
4. **Blazor Full Stack** - SSR + Interactive components

**Khuyến nghị áp dụng cho dự án Northwind:**
- ✅ Sử dụng Primary Constructors cho tất cả Services
- ✅ Áp dụng Collection Expressions cho cleaner code
- ✅ Enable Native AOT cho Web API (production)
- ✅ Sử dụng Keyed Services DI cho versioning API
- ✅ Migrate sang OpenAPI built-in thay vì Swashbuckle
- ✅ Enable HTTP/3 cho performance tốt hơn

### Lộ trình học tập đề xuất

**Nếu bạn là sinh viên năm 2-3:**
- Focus vào Tuần 1-3 (WinForm + ADO.NET)
- Học thêm Tuần 4-5 (EF Core + LINQ) nếu có thời gian

**Nếu bạn là sinh viên năm 4 hoặc thực tập:**
- Hoàn thành đầy đủ Tuần 1-7
- Thêm Unit Testing và Deploy Azure (Bonus)

**Nếu bạn muốn trở thành Full-Stack Developer:**
- Hoàn thành tất cả 7 tuần
- Học thêm Level 1-2 (Microservices)
- Thực hành Level 4 (Modern Frontend)

**Nếu bạn hướng đến DevOps/Cloud Engineer:**
- Hoàn thành Tuần 1-7
- Focus vào Level 3 (Cloud-Native)
- Học Docker, Kubernetes, CI/CD

### Tài liệu tham khảo

**Official Documentation:**
- [Microsoft Learn - C# và .NET](https://learn.microsoft.com/dotnet/)
- [Entity Framework Core](https://learn.microsoft.com/ef/core/)
- [ASP.NET Core](https://learn.microsoft.com/aspnet/core/)
- [Azure Documentation](https://learn.microsoft.com/azure/)

**Books:**
- "Pro C# 10 with .NET 6" - Andrew Troelsen
- "Entity Framework Core in Action" - Jon P Smith
- "Building Microservices" - Sam Newman
- "Clean Architecture" - Robert C. Martin

**Online Courses:**
- [Pluralsight - .NET Development](https://www.pluralsight.com/paths/csharp)
- [Udemy - ASP.NET Core MVC](https://www.udemy.com/topic/asp-net-core/)
- [Microsoft Learn](https://learn.microsoft.com)

---

**Chúc bạn thành công với dự án! 🚀**

---

## 📝 GHI CHÚ QUAN TRỌNG VỀ .NET 10

### Yêu cầu hệ thống

**Development Machine:**
- **Visual Studio 2022** (Version 17.12 trở lên) hoặc **Visual Studio Code** với C# Dev Kit
- **.NET 10 SDK** - Download từ: https://dotnet.microsoft.com/download/dotnet/10.0
- **SQL Server 2019/2022** (Express, Developer, hoặc Standard Edition)
- **SQL Server Management Studio (SSMS)** phiên bản mới nhất

**Kiểm tra version:**
```bash
dotnet --version
# Phải hiển thị: 10.0.x

dotnet --list-sdks
# Phải có: 10.0.x

dotnet --list-runtimes
# Phải có: Microsoft.AspNetCore.App 10.0.x
#          Microsoft.NETCore.App 10.0.x
```

### Cài đặt .NET 10

```bash
# Windows - Download installer từ Microsoft
# https://dotnet.microsoft.com/download/dotnet/10.0

# macOS - Homebrew
brew install dotnet@10

# Linux (Ubuntu/Debian)
wget https://dotnet.microsoft.com/download/dotnet/scripts/v1/dotnet-install.sh
chmod +x dotnet-install.sh
./dotnet-install.sh --channel 10.0

# Verify installation
dotnet --info
```

### Tạo project .NET 10

```bash
# WinForms App (.NET 10)
dotnet new winforms -n NorthwindWinForms -f net10.0

# Web API (.NET 10)
dotnet new webapi -n NorthwindAPI -f net10.0

# MVC Web App (.NET 10)
dotnet new mvc -n NorthwindMVC -f net10.0

# Class Library (.NET 10)
dotnet new classlib -n NorthwindDAL -f net10.0
```

### Performance Benefits của .NET 10

Compared to .NET 8 (.NET 6/7/8):
- ⚡ **JSON Serialization**: 30% nhanh hơn
- ⚡ **EF Core Queries**: 20% nhanh hơn
- ⚡ **Startup Time**: Giảm 40% với Native AOT
- ⚡ **Memory Usage**: Giảm 25% cho Web API
- ⚡ **HTTP/3**: Default enabled, latency thấp hơn
- ⚡ **LINQ Performance**: Optimized allocations

### Migration từ .NET 8 lên .NET 10

Nếu bạn có project .NET 8:
```xml
<!-- Thay đổi trong .csproj -->
<TargetFramework>net8.0</TargetFramework>
<!-- Thành -->
<TargetFramework>net10.0</TargetFramework>
```

Sau đó update packages:
```bash
dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 10.0.0
dotnet add package Microsoft.AspNetCore.Authentication.JwtBearer --version 10.0.0
```

### Compatibility

**.NET 10 tương thích với:**
- ✅ SQL Server 2019/2022
- ✅ Azure SQL Database
- ✅ PostgreSQL 12+
- ✅ MySQL 8.0+
- ✅ SQLite 3.x
- ✅ Docker containers
- ✅ Kubernetes
- ✅ Azure App Service
- ✅ AWS Elastic Beanstalk
- ✅ Google Cloud Run

### Troubleshooting

**Nếu gặp lỗi "SDK not found":**
```bash
# Kiểm tra PATH environment variable
echo $PATH  # Linux/macOS
echo %PATH%  # Windows

# Thêm .NET vào PATH nếu cần
export PATH=$PATH:/usr/local/share/dotnet  # Linux/macOS
```

**Nếu Visual Studio không nhận .NET 10:**
- Update Visual Studio 2022 lên version mới nhất (17.12+)
- Tools → Options → Preview Features → Enable previews of the .NET SDK

---

*Lưu ý: Kế hoạch này được thiết kế dựa trên đề cương môn Lập trình Cơ sở Dữ liệu (ITEC3406) của Đại học Mở TP.HCM, được cập nhật để sử dụng **.NET 10** (phiên bản mới nhất), phù hợp cho sinh viên từ năm 2 đến năm 4 và những người muốn học lập trình C# .NET từ cơ bản đến nâng cao với công nghệ hiện đại nhất.*

**Version:** 2.0 - Updated for .NET 10 & C# 13
**Last Updated:** 2025-01-18
