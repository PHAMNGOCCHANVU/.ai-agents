# NGỮ CẢNH ĐẶC THÙ CỦA DỰ ÁN (PROJECT-SPECIFIC OVERLAY)
> **Hướng dẫn sử dụng:** Đây là file duy nhất bạn cần chỉnh sửa khi bắt đầu một dự án mới.

## 1. Thông tin chung & Kiến trúc
- **Tên dự án:** [Điền tên dự án, VD: E-Commerce Microservices / AI RAG System]
- **Mô hình kiến trúc:** [VD: Clean Architecture / Modular Monolith / Microservices]
- **Ngôn ngữ chính & Framework:**
  - **Backend:** [VD: Go 1.23 (Gin/Gorm) HOẶC Python 3.12 (FastAPI) HOẶC PHP (Laravel)]
  - **Frontend:** [VD: Next.js 15, TypeScript, TailwindCSS]
  - **Database & Cache:** [VD: PostgreSQL 16, Redis 7, Qdrant Vector DB]

---

## 2. Phân quyền Thư mục cho từng Tác nhân (File Ownership Boundaries)
Các Agent chỉ có quyền **GHI (Write/Modify)** trong các đường dẫn được cấp phép dưới đây:

| Tác nhân (Role) | Đường dẫn được phép GHI (Allowed Write Paths) | Đường dẫn CẤM GHI (Forbidden Paths) |
| :--- | :--- | :--- |
| **Architect Agent** | `docs/specs/**`, `docs/architecture/**`, `openapi.yaml` | Toàn bộ thư mục mã nguồn và cấu hình |
| **Backend Agent** | `backend/**`, `internal/**`, `pkg/**`, `migrations/**` | `frontend/**`, `tests/e2e/**`, `.github/**` |
| **Frontend Agent** | `frontend/**`, `web/**`, `public/**` | `backend/**`, `internal/**`, `.github/**` |
| **QA / Tester Agent** | `tests/**`, `**/*_test.go`, `**/*.spec.ts`, `**/*.test.py` | Toàn bộ code nghiệp vụ (chỉ được đọc) |
| **DevOps Agent** | `.github/workflows/**`, `docker/**`, `Dockerfile*`, `docker-compose*.yml`, `Makefile` | Mã nguồn nghiệp vụ và thư mục test |

---

## 3. Bộ lệnh Tiêu chuẩn (Standard CLI Commands)
Các Agent phải sử dụng đúng các câu lệnh dưới đây để tự kiểm tra kết quả:

- **Kiểm tra Linter / Format:**
  - Backend: `[VD: golangci-lint run ./... HOẶC ruff check .]`
  - Frontend: `[VD: npm run lint && npm run type-check]`
- **Chạy Unit / Integration Test:**
  - Backend: `[VD: go test -v -race ./... HOẶC pytest]`
  - Frontend: `[VD: npm run test]`
- **Khởi chạy môi trường Local:**
  - `[VD: docker compose up -d --build]`

---

## 4. Quy tắc Nghiệp vụ & Kỹ thuật Đặc thù (Domain Rules)
*(Bổ sung các lưu ý riêng của dự án này để Agent tuân thủ)*
- **Xử lý đồng thời / Bất đồng bộ:** [VD: Mọi API tạo đơn hàng phải có Idempotency Key lưu trên Redis trong 24h].
- **Chuẩn trả về API:** [VD: Luôn trả về cấu trúc JSON thống nhất `{ "success": bool, "data": any, "error": { "code": string, "message": string } }`].
- **Quy ước Database:** [VD: Không dùng khóa ngoại vật lý nếu chia shard, mọi bảng phải có `created_at`, `updated_at` dạng UTC].