Giải thích cấu trúc thư mục và hướng dẫn sao chép `appsettings.Development.json`

Mục đích
- Tài liệu này giải thích ngắn gọn cấu trúc thư mục của project để người không quen với ASP.NET có thể hiểu nhanh, và hướng dẫn cách sao chép file cấu hình phát triển an toàn từ file ví dụ.

Cấu trúc chính 
- Thư mục gốc của backend (`FeedVetStoreManagementAPI/`) — chứa mã nguồn của API (ứng dụng server).
- `Program.cs` — điểm khởi động của ứng dụng. Nghĩ đơn giản như công tắc bật/tắt và nơi kết nối các thành phần (cơ sở dữ liệu, xác thực, định tuyến).
- `appsettings.json` — cấu hình chung dùng cho mọi môi trường (giá trị mặc định không chứa bí mật môi trường).
- `appsettings.Development.json` — cấu hình ghi đè khi chạy ở môi trường Development (ví dụ: chuỗi kết nối cơ sở dữ liệu cục bộ, tùy chọn debug).
- `appsettings.Development.example.json` — file mẫu an toàn để chia sẻ; chứa cấu trúc và giá trị mẫu, không chứa bí mật thật. Bạn sao chép file này để tạo `appsettings.Development.json` và điền giá trị cục bộ.
- `Data/AppDbContext.cs` — định nghĩa cách ứng dụng kết nối và tương tác với cơ sở dữ liệu (bảng, quan hệ).
- `Models/` — chứa các lớp dữ liệu (mô hình) và enum mà ứng dụng dùng để truyền dữ liệu.
- `Controllers/` — (nếu có) chứa các endpoint HTTP mà client gọi để lấy hoặc lưu dữ liệu.
- `Services/` — chứa các lớp dịch vụ xử lý logic nghiệp vụ (ví dụ: gửi email, xử lý thanh toán).
- `Repositories/` — chứa các lớp truy cập dữ liệu, tương tác trực tiếp với cơ sở dữ liệu.
- `Migrations/` — chứa các file di trú cơ sở dữ liệu do Entity Framework tạo ra để theo dõi thay đổi cấu trúc DB.
- `Properties/launchSettings.json` — cấu hình chạy ứng dụng trong môi trường phát triển (IDE như Visual Studio).
- `wwwroot/` — chứa các tài nguyên tĩnh (nếu có) như hình ảnh, CSS, JavaScript phục vụ cho frontend.
- `FeedVetStoreManagementAPI.Test/` — chứa các bài kiểm thử tự động để kiểm tra hành vi của ứng dụng.

Cách hoạt động của cấu hình theo môi trường (ngôn ngữ dễ hiểu)
- Ứng dụng đọc `appsettings.json` trước, sau đó đọc `appsettings.{Environment}.json` (ví dụ `appsettings.Development.json`) để ghi đè các giá trị khi biến môi trường `ASPNETCORE_ENVIRONMENT` đặt là `Development`.
- Điều này cho phép giữ cấu hình production riêng biệt và an toàn, đồng thời sử dụng cấu hình phù hợp khi phát triển trên máy cá nhân.

Hướng dẫn an toàn: sao chép file cấu hình phát triển
1. Copy file mẫu thành file cấu hình phát triển thực tế
- PowerShell:
  - `Copy-Item "FeedVetStoreManagementAPI\\appsettings.Development.example.json" "FeedVetStoreManagementAPI\\appsettings.Development.json"`
- Command Prompt (cmd):
  - `copy "FeedVetStoreManagementAPI\\appsettings.Development.example.json" "FeedVetStoreManagementAPI\\appsettings.Development.json"`
- Git Bash / WSL / Linux:
  - `cp FeedVetStoreManagementAPI/appsettings.Development.example.json FeedVetStoreManagementAPI/appsettings.Development.json`
- Hoặc mở file `appsettings.Development.example.json` trong trình soạn thảo và lưu lại thành `appsettings.Development.json`.

2. Chỉnh sửa `appsettings.Development.json`
- Thay placeholder bằng chuỗi kết nối cơ sở dữ liệu cục bộ, secret JWT, API key cho dịch vụ bên thứ ba dùng cho phát triển.

3. Không commit bí mật vào Git
- Đảm bảo file `appsettings.Development.json` không bị đưa lên kho mã công khai. Nếu repository chưa có, thêm dòng sau vào `.gitignore`:
  - `FeedVetStoreManagementAPI/appsettings.Development.json`
- Bạn có thể thêm bằng lệnh: `echo "FeedVetStoreManagementAPI/appsettings.Development.json" >> .gitignore`

4. Chạy ứng dụng ở môi trường Development
- Thiết lập biến môi trường trước khi chạy nếu cần:
  - PowerShell: `$env:ASPNETCORE_ENVIRONMENT = "Development"`
  - Command Prompt: `set ASPNETCORE_ENVIRONMENT=Development`
- Visual Studio / IDE thường đã đặt `ASPNETCORE_ENVIRONMENT` trong `launchSettings.json` cho profile Debug.
- Khởi động ứng dụng: chạy `dotnet run` trong thư mục `FeedVetStoreManagementAPI` hoặc dùng IDE.


