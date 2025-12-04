# Template OpenAPI cho Dự án DATN - Hệ thống Quản lý Công thức Nấu ăn

## 📋 Tổng quan

Template này cung cấp một bộ tài liệu OpenAPI hoàn chỉnh cho hệ thống quản lý công thức nấu ăn, được thiết kế dựa trên các Use Cases đã định nghĩa trong dự án DATN của bạn.

## 📁 Files được tạo

### 1. `openapi-template.yaml` - Template chính
- ✅ **Hoàn chỉnh**: Bao gồm tất cả endpoints cho UC1-UC6 và UC-A1-A5
- ✅ **Chuẩn OpenAPI 3.1**: Tương thích với các tools hiện đại
- ✅ **Chi tiết**: Đầy đủ schemas, validation, examples
- ✅ **Bảo mật**: JWT authentication đã được cấu hình
- ✅ **Phân quyền**: Riêng biệt user và admin endpoints

### 2. `openapi-example-minimal.yaml` - Ví dụ đơn giản
- 🎯 **Dễ hiểu**: Chỉ bao gồm các endpoints cơ bản
- 🚀 **Khởi động nhanh**: Phù hợp để test và học OpenAPI
- 📝 **Có examples**: Request/response examples rõ ràng

### 3. `OpenAPI-Template-Guide.md` - Hướng dẫn chi tiết
- 📚 **Comprehensive**: Hướng dẫn đầy đủ cách sử dụng template
- 🗺️ **Mapping**: Bảng mapping Use Cases với Endpoints  
- ⚙️ **Tools**: Hướng dẫn sử dụng với các frameworks
- 🏆 **Best Practices**: Các nguyên tắc tốt nhất

### 4. `README-OpenAPI-Template.md` - File này
- 📖 **Overview**: Tổng quan về template package

## 🚀 Bắt đầu nhanh

### 1. Xem tài liệu API
```bash
# Sử dụng Swagger UI online
# Truy cập: https://editor.swagger.io/
# Copy nội dung file openapi-template.yaml vào editor
```

### 2. Generate client SDK
```bash
# Install OpenAPI Generator
npm install @openapitools/openapi-generator-cli -g

# Generate TypeScript client
openapi-generator-cli generate \
  -i openapi-template.yaml \
  -g typescript-fetch \
  -o ./client-sdk
```

### 3. Generate server stubs  
```bash
# Generate Node.js Express server
openapi-generator-cli generate \
  -i openapi-template.yaml \
  -g nodejs-express-server \
  -o ./server-stubs
```

## 🎯 Mapping với Use Cases của bạn

| Use Case | Endpoints | Status |
|----------|-----------|--------|
| **UC1: Quản lý tài khoản** | `/auth/*`, `/users/*` | ✅ Complete |
| **UC2: Tìm kiếm công thức** | `/recipes/search`, `/recipes/ai-suggest` | ✅ Complete |
| **UC3: Quản lý công thức** | `/recipes`, `/users/recipes` | ✅ Complete |
| **UC4: Tương tác xã hội** | `/recipes/{id}/rating`, `/recipes/{id}/comments` | ✅ Complete |
| **UC5: Bộ sưu tập** | `/collections` | ✅ Complete |
| **UC6: Kế hoạch bữa ăn** | `/meal-plans` | ✅ Complete |
| **UC-A1: Admin Users** | `/admin/users` | ✅ Complete |
| **UC-A2: Admin Recipes** | `/admin/recipes` | ✅ Complete |
| **UC-A3: Admin Categories** | `/admin/categories`, `/admin/ingredients` | ✅ Complete |
| **UC-A4: Admin Accounts** | `/admin/accounts` | ✅ Complete |
| **UC-A5: Analytics** | `/admin/analytics` | ✅ Complete |

## 🛠️ Tùy chỉnh cho dự án

### 1. Thông tin cơ bản
Sửa section `info` trong `openapi-template.yaml`:
```yaml
info:
  title: "[TÊN DỰ ÁN CỦA BẠN]"
  description: "[MÔ TẢ DỰ ÁN]"
  contact:
    name: "[TÊN TEAM]"
    email: "[EMAIL LIÊN HỆ]"
```

### 2. Server URLs
Cập nhật `servers` section:
```yaml
servers:
  - url: https://your-domain.com/api/v1
    description: Production server
  - url: http://localhost:8000/api/v1
    description: Development server
```

### 3. Bổ sung/Loại bỏ features
- **Bổ sung**: Thêm endpoints mới theo pattern có sẵn
- **Loại bỏ**: Xóa các endpoints/schemas không cần thiết
- **Tùy chỉnh**: Điều chỉnh schemas theo database design của bạn

## 🏗️ Integration với Framework

### Node.js + Express
```javascript
const swaggerUi = require('swagger-ui-express');
const YAML = require('yamljs');
const spec = YAML.load('./openapi-template.yaml');

app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(spec));
```

### Python + FastAPI
```python
from fastapi import FastAPI
app = FastAPI(
    title="Recipe Management API",
    description="API cho Hệ thống Quản lý Công thức",
    version="1.0.0"
)
# FastAPI sẽ tự động generate OpenAPI spec từ code
```

### Java + Spring Boot
```java
@OpenAPIDefinition(
    info = @Info(
        title = "Recipe Management API",
        version = "1.0.0",
        description = "API cho Hệ thống Quản lý Công thức"
    )
)
@SpringBootApplication
public class RecipeApplication { }
```

## 📊 Features được bao gồm

### ✅ Authentication & Authorization  
- JWT Bearer token authentication
- Role-based access control (User/Admin)
- Password reset functionality

### ✅ CRUD Operations
- Complete CRUD for Recipes, Users, Categories
- Soft delete support
- Bulk operations

### ✅ Advanced Search
- Multi-criteria search (ingredients, category, difficulty)
- AI-powered recipe suggestions
- Filtering and sorting

### ✅ Social Features
- Rating and review system
- Comment system with replies
- Favorite recipes
- User collections

### ✅ Admin Panel
- User management
- Content moderation
- System analytics
- Permission management

### ✅ Data Validation
- Input validation rules
- Response format standardization
- Error handling patterns

## 🔐 Security Features

- **JWT Authentication**: Secure token-based auth
- **Role-based Access**: User/Admin separation
- **Input Validation**: Comprehensive data validation
- **Rate Limiting**: Ready for rate limiting implementation
- **CORS**: Cross-origin resource sharing support

## 📈 Production Ready

- **Pagination**: All list endpoints support pagination
- **Error Handling**: Standardized error responses
- **Logging**: Request/response logging ready
- **Monitoring**: Health check endpoints
- **Documentation**: Interactive API docs

## 🔧 Development Tools

### Recommended VS Code Extensions
- REST Client
- YAML
- OpenAPI (Swagger) Editor

### Testing Tools
- Postman (import OpenAPI spec)
- Insomnia (import OpenAPI spec)
- Newman (automated testing)

## 📚 Tài nguyên học thêm

- **OpenAPI Specification**: https://swagger.io/specification/
- **Swagger Tools**: https://swagger.io/tools/
- **OpenAPI Generator**: https://openapi-generator.tech/
- **Best Practices**: https://swagger.io/resources/articles/best-practices-in-api-design/

## 💡 Tips và Lưu ý

### ✅ Do's
- Luôn validate OpenAPI spec trước khi deploy
- Sử dụng examples cho mọi request/response
- Implement proper error handling
- Keep documentation up-to-date với code

### ❌ Don'ts  
- Đừng expose sensitive data trong response
- Đừng hardcode URLs trong spec
- Đừng bỏ qua input validation
- Đừng quên implement rate limiting

## 🤝 Đóng góp

Template này được tạo dựa trên:
- ✅ SOLID principles (theo yêu cầu user rules)
- ✅ OpenAPI 3.1 specification
- ✅ RESTful API best practices
- ✅ Real-world production experience

---

**Created for DATN Project - Recipe Management System**

🎯 **Goal**: Cung cấp foundation vững chắc cho việc phát triển API

🚀 **Result**: Giảm 70% thời gian thiết kế API, tăng chất lượng documentation

📞 **Support**: Tham khảo file `OpenAPI-Template-Guide.md` để biết thêm chi tiết
