# KẾ HOẠCH PHÁT TRIỂN ỨNG DỤNG QUẢN LÝ NORTHWIND

**Công nghệ sử dụng:** WinForm C# + SQL Server
**Cơ sở dữ liệu:** Northwind Database
**Thời gian thực hiện:** 4 tuần

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

### Công nghệ và thư viện sử dụng

**Framework:**
- .NET Framework 4.7.2 hoặc cao hơn
- C# WinForms

**Database:**
- SQL Server 2014 trở lên
- ADO.NET hoặc Entity Framework

**Thư viện bổ sung:**
- **EPPlus** hoặc **ClosedXML**: Xuất Excel
- **System.Windows.Forms.DataVisualization**: Biểu đồ
- **iTextSharp** (tùy chọn): Xuất PDF

### Tiêu chí đánh giá

1. **Chức năng (40%)**
   - Đầy đủ các chức năng CRUD
   - Tìm kiếm hoạt động chính xác
   - Thống kê và báo cáo đúng dữ liệu

2. **Giao diện (20%)**
   - Thân thiện, dễ sử dụng
   - Bố cục hợp lý
   - Responsive khi resize

3. **Code quality (20%)**
   - Kiến trúc 3-layer rõ ràng
   - Code sạch, có comment
   - Xử lý exception tốt

4. **Database (20%)**
   - Thiết kế chuẩn hóa
   - Có ràng buộc và index
   - Stored procedures (nếu có)

### Ghi chú quan trọng

⚠️ **Lưu ý khi phát triển:**
- Backup database thường xuyên
- Test kỹ trước khi chuyển sang tuần tiếp theo
- Commit code lên Git sau mỗi tính năng hoàn thành
- Viết tài liệu hướng dẫn sử dụng

💡 **Gợi ý mở rộng:**
- Thêm chức năng đăng nhập và phân quyền
- Gửi email thông báo đơn hàng
- Tích hợp API thanh toán
- Làm báo cáo Crystal Report
- Chuyển sang WPF hoặc Web Application

---

**Chúc bạn thành công với dự án! 🚀**
