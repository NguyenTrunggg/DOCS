# Hướng dẫn sử dụng Template OpenAPI

## Giới thiệu

Template OpenAPI này được thiết kế đặc biệt cho **Hệ thống Quản lý Công thức Nấu ăn** trong dự án DATN. Template bao gồm tất cả các endpoint cần thiết dựa trên các Use Cases đã định nghĩa từ UC1 đến UC6 và UC-A1 đến UC-A5.

## Cấu trúc Template

### 1. Thông tin cơ bản (Info Section)
```yaml
openapi: 3.1.0
info:
  title: Recipe Management System API
  description: API cho Hệ thống Quản lý Công thức Nấu ăn
  version: 1.0.0
```

### 2. Servers Configuration
- **Production**: `https://api.recipemanagement.com/v1`
- **Staging**: `https://staging-api.recipemanagement.com/v1`  
- **Development**: `http://localhost:3000/api/v1`

### 3. Tags Organization
Template được tổ chức theo 13 tags chính:
- Authentication (Xác thực)
- User Management (Quản lý người dùng)
- Recipe Search (Tìm kiếm công thức)
- Recipe Management (Quản lý công thức)
- Social Features (Tính năng xã hội)
- Collections (Bộ sưu tập)
- Planning (Lập kế hoạch)
- Admin modules (5 tags quản trị)

## Mapping Use Cases với Endpoints

### UC1: Quản lý tài khoản người dùng
| Use Case | Endpoint | Method | Description |
|----------|----------|--------|-------------|
| UCS01-1 | `/auth/register` | POST | Đăng ký tài khoản |
| UCS01-2 | `/auth/login` | POST | Đăng nhập hệ thống |
| UCS01-3 | `/auth/logout` | POST | Đăng xuất |
| UCS01-4 | `/users/profile` | GET | Xem thông tin cá nhân |
| UCS01-5 | `/users/profile` | PUT | Cập nhật thông tin |
| UCS01-6 | `/users/change-password` | PUT | Đổi mật khẩu |
| UCS01-7 | `/auth/forgot-password` | POST | Đặt lại mật khẩu |

### UC2: Tìm kiếm và gợi ý công thức
| Use Case | Endpoint | Method | Description |
|----------|----------|--------|-------------|
| UCS02-1 | `/recipes/search?ingredients=...` | GET | Tìm theo nguyên liệu |
| UCS02-2 | `/recipes/search?q=...` | GET | Tìm theo tên món |
| UCS02-3 | `/recipes/ai-suggest` | POST | Gợi ý bằng AI |
| UCS02-4 | `/recipes/search?sortBy=...` | GET | Lọc và sắp xếp |
| UCS02-5 | `/recipes/{id}` | GET | Xem chi tiết |

### UC3: Quản lý công thức cá nhân
| Use Case | Endpoint | Method | Description |
|----------|----------|--------|-------------|
| UCS03-1 | `/recipes` | POST | Thêm công thức mới |
| UCS03-2 | `/users/recipes` | GET | Danh sách công thức |
| UCS03-3 | `/recipes/{id}/edit` | PUT | Chỉnh sửa |
| UCS03-4 | `/recipes/{id}/edit` | DELETE | Xóa công thức |

### UC4-UC6: Tính năng xã hội và quản lý
- **UC4**: Rating, Comments, Favorites (`/recipes/{id}/rating`, `/recipes/{id}/comments`, `/recipes/{id}/favorite`)
- **UC5**: Collections (`/collections`)
- **UC6**: Meal Planning (`/meal-plans`)

### Admin Use Cases (UC-A1 đến UC-A5)
- **UCA01**: User Management (`/admin/users`)
- **UCA02**: Recipe System Management (`/admin/recipes`)
- **UCA03**: Category & Ingredient Management (`/admin/categories`, `/admin/ingredients`)
- **UCA04**: Admin Account Management (`/admin/accounts`)
- **UCA05**: Analytics (`/admin/analytics`)

## Schema Design Principles

### 1. Tái sử dụng với $ref
```yaml
# Thay vì định nghĩa lại, sử dụng reference
user:
  $ref: '#/components/schemas/User'
```

### 2. Validation Rules
```yaml
password:
  type: string
  minLength: 8
  description: Mật khẩu (ít nhất 8 ký tự)
```

### 3. Enums cho giá trị cố định
```yaml
difficulty:
  type: string
  enum: [easy, medium, hard]
```

## Security Implementation

### JWT Authentication
```yaml
securitySchemes:
  bearerAuth:
    type: http
    scheme: bearer
    bearerFormat: JWT
```

### Áp dụng security
```yaml
# Global security
security:
  - bearerAuth: []

# Endpoint-specific (override global)
/auth/register:
  post:
    # Không cần auth cho đăng ký
```

## Response Standardization

### Success Response
```yaml
SuccessResponse:
  type: object
  properties:
    success:
      type: boolean
      example: true
    message:
      type: string
    data:
      type: object
      nullable: true
```

### Error Response
```yaml
ErrorResponse:
  type: object
  properties:
    success:
      type: boolean
      example: false
    error:
      type: object
      properties:
        code:
          type: string
        message:
          type: string
```

### Pagination Pattern
```yaml
Pagination:
  type: object
  properties:
    page: 
      type: integer
    limit:
      type: integer  
    total:
      type: integer
    totalPages:
      type: integer
    hasNext:
      type: boolean
    hasPrevious:
      type: boolean
```

## Cách sử dụng Template

### 1. Tùy chỉnh thông tin dự án
```yaml
info:
  title: "Tên dự án của bạn"
  description: "Mô tả dự án"
  contact:
    name: "Tên team"
    email: "email@yourproject.com"
```

### 2. Cập nhật servers
```yaml
servers:
  - url: https://your-api-domain.com/v1
    description: Production server
```

### 3. Thêm/Bớt endpoints theo yêu cầu
- Giữ lại những endpoints cần thiết
- Bổ sung thêm endpoints đặc biệt cho dự án
- Xóa bỏ những phần không sử dụng

### 4. Tùy chỉnh schemas
- Thêm/bớt fields trong các model
- Cập nhật validation rules
- Điều chỉnh relationships

## Tools và Workflow

### 1. Swagger Editor
- Truy cập: https://editor.swagger.io/
- Import file `openapi-template.yaml`
- Chỉnh sửa trực tiếp với live preview

### 2. Code Generation
```bash
# Install OpenAPI Generator
npm install @openapitools/openapi-generator-cli -g

# Generate client SDK
openapi-generator-cli generate -i openapi-template.yaml -g typescript-fetch -o ./client-sdk

# Generate server stubs
openapi-generator-cli generate -i openapi-template.yaml -g nodejs-express-server -o ./server-stubs
```

### 3. Documentation Generation
```bash
# Generate HTML docs with Redoc
npx redoc-cli build openapi-template.yaml --output docs.html
```

## Best Practices khi sử dụng

### 1. Versioning API
```yaml
# Trong URL
servers:
  - url: https://api.example.com/v1

# Hoặc trong header
info:
  version: "1.0.0"
```

### 2. Error Handling
```yaml
responses:
  '400':
    $ref: '#/components/responses/BadRequest'
  '401':
    $ref: '#/components/responses/Unauthorized'
  '403':
    $ref: '#/components/responses/Forbidden'
```

### 3. Request/Response Examples
```yaml
requestBody:
  content:
    application/json:
      schema:
        $ref: '#/components/schemas/LoginRequest'
      example:
        email: "user@example.com"
        password: "SecurePass123!"
```

### 4. Parameter Reuse
```yaml
components:
  parameters:
    PageParam:
      name: page
      in: query
      schema:
        type: integer
        minimum: 1
        default: 1
```

## Integration với Backend Framework

### Node.js + Express
```javascript
const swaggerUi = require('swagger-ui-express');
const YAML = require('yamljs');
const swaggerDocument = YAML.load('./openapi-template.yaml');

app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(swaggerDocument));
```

### Python + FastAPI
```python
from fastapi import FastAPI
from fastapi.openapi.utils import get_openapi

app = FastAPI()

def custom_openapi():
    if app.openapi_schema:
        return app.openapi_schema
    openapi_schema = get_openapi(
        title="Recipe Management System API",
        version="1.0.0",
        description="API cho Hệ thống Quản lý Công thức Nấu ăn",
        routes=app.routes,
    )
    app.openapi_schema = openapi_schema
    return app.openapi_schema

app.openapi = custom_openapi
```

## Testing API với OpenAPI

### 1. Postman Collection
- Import OpenAPI spec vào Postman
- Auto-generate test collection
- Setup environment variables

### 2. Contract Testing
```javascript
// Sử dụng swagger-parser để validate
const SwaggerParser = require('swagger-parser');

async function validateAPI() {
  try {
    await SwaggerParser.validate('./openapi-template.yaml');
    console.log('API specification is valid!');
  } catch (err) {
    console.error('Validation failed:', err);
  }
}
```

## Lưu ý quan trọng

### 1. Bảo mật
- Luôn sử dụng HTTPS cho production
- Implement rate limiting
- Validate tất cả input data
- Không expose sensitive data trong response

### 2. Performance  
- Implement pagination cho danh sách lớn
- Sử dụng caching cho data không thay đổi thường xuyên
- Optimize database queries

### 3. Maintainability
- Giữ documentation luôn cập nhật với code
- Sử dụng semantic versioning
- Implement backward compatibility

## Kết luận

Template OpenAPI này cung cấp foundation vững chắc cho việc phát triển API của hệ thống quản lý công thức nấu ăn. Hãy tùy chỉnh theo nhu cầu cụ thể của dự án và tuân theo các best practices để đảm bảo API có chất lượng cao.

---

**Tài liệu tham khảo:**
- [OpenAPI Specification](https://swagger.io/specification/)
- [Swagger Tools](https://swagger.io/tools/)
- [OpenAPI Generator](https://openapi-generator.tech/)
