# OptibuildHub

Ứng dụng web hỗ trợ người dùng **xây dựng cấu hình PC**, **so sánh linh kiện**, **theo dõi giá** và **thảo luận** trên diễn đàn cộng đồng.

> **Demo:** [https://optibuild-hub.vercel.app](https://optibuild-hub.vercel.app/)
>
> **Source code:**
> - Frontend: [https://github.com/xuannhat1503/OptibuildHub_Frontend](https://github.com/xuannhat1503/OptibuildHub_Frontend)
> - Backend: [https://github.com/xuannhat1503/OptibuildHub_Backend](https://github.com/xuannhat1503/OptibuildHub_Backend)

---

## Mục lục

- [Tính năng](#tính-năng)
- [Công nghệ sử dụng](#công-nghệ-sử-dụng)
- [Yêu cầu hệ thống](#yêu-cầu-hệ-thống)
- [Cài đặt Backend](#cài-đặt-backend)
- [Cài đặt Frontend](#cài-đặt-frontend)
- [Biến môi trường](#biến-môi-trường)
- [Deploy với Docker](#deploy-với-docker)

---

## Tính năng

### Linh kiện PC
- Duyệt và tìm kiếm linh kiện theo danh mục: **CPU, GPU, RAM, Mainboard, PSU, Case, Storage, Cooling**
- Lọc và sắp xếp theo giá, tên, ngày thêm
- Xem chi tiết thông số kỹ thuật từng linh kiện
- Theo dõi **lịch sử giá** theo thời gian
- **Crawl giá tự động** từ các trang thương mại điện tử (Jsoup)

### PC Builder
- Chọn linh kiện cho từng slot để tạo cấu hình PC hoàn chỉnh
- **Kiểm tra tương thích** tự động giữa các linh kiện (CPU socket, RAM type, v.v.)
- Lưu, chỉnh sửa và quản lý nhiều bản cấu hình

### So sánh linh kiện
- So sánh song song thông số kỹ thuật của nhiều linh kiện cùng loại

### Đánh giá
- Đánh giá sao cho linh kiện
- Xem điểm đánh giá trung bình từ cộng đồng

### Diễn đàn
- Tạo bài đăng kèm hình ảnh
- Bình luận và phản hồi
- Reaction (thả cảm xúc) cho bài viết
- Phân trang bài đăng

### Xác thực & Phân quyền
- Đăng ký, đăng nhập bằng JWT
- Hai vai trò: **USER** và **ADMIN**
- Trang quản trị dành riêng cho Admin

### Upload File
- Upload ảnh cho bài đăng diễn đàn và avatar
- Giới hạn file: tối đa 20MB/file

---

## Công nghệ sử dụng

| Phần | Công nghệ |
|---|---|
| **Backend** | Java 17, Spring Boot 3.1, Spring Security, Spring Data JPA |
| **Database** | MySQL 8 |
| **Authentication** | JWT (jjwt 0.11.5) |
| **Web Crawler** | Jsoup 1.17 |
| **Frontend** | React 19, Vite 7, Tailwind CSS 4 |
| **HTTP Client** | Axios |
| **Routing** | React Router DOM 7 |
| **Deploy** | Docker (Backend), Vercel (Frontend) |

---

## Yêu cầu hệ thống

- **Java** 17+
- **Maven** 3.8+ (hoặc dùng `mvnw` đi kèm)
- **MySQL** 8.0+
- **Node.js** 18+ và **npm** 9+

---

## Cài đặt Backend

### 1. Clone repository

```bash
git clone https://github.com/xuannhat1503/OptibuildHub_Backend.git
cd OptibuildHub_Backend
```

### 2. Tạo database MySQL

```sql
CREATE DATABASE optibuild_hub CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

### 3. Cấu hình kết nối database

Mở file `src/main/resources/application.properties` và chỉnh sửa:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/optibuild_hub?useSSL=false&serverTimezone=Asia/Ho_Chi_Minh
spring.datasource.username=root
spring.datasource.password=YOUR_PASSWORD
```

### 4. Cấu hình JWT (tùy chọn)

```properties
jwt.secret=your-secret-key-at-least-256-bits
jwt.expiration=86400000
```

### 5. Chạy ứng dụng

```bash
# Dùng Maven Wrapper (không cần cài Maven sẵn)
./mvnw spring-boot:run

# Hoặc build JAR rồi chạy
./mvnw package -DskipTests
java -jar target/optibuild_hub-1.0-SNAPSHOT.jar
```

Backend sẽ khởi động tại: `http://localhost:8080`

> **Lưu ý:** Hibernate sẽ tự động tạo/cập nhật bảng (`ddl-auto=update`) khi ứng dụng khởi động lần đầu.

---

## Cài đặt Frontend

### 1. Clone repository

```bash
git clone https://github.com/xuannhat1503/OptibuildHub_Frontend.git
cd OptibuildHub_Frontend
```

### 2. Cài đặt dependencies

```bash
npm install
```

### 3. Cấu hình API URL

Tạo file `.env` trong thư mục `OptibuildHub_Frontend`:

```env
VITE_API_URL=http://localhost:8080
```

> Nếu không tạo file `.env`, frontend sẽ mặc định kết nối đến `http://localhost:8080`.

### 4. Chạy môi trường development

```bash
npm run dev
```

Frontend sẽ khởi động tại: `http://localhost:5173`

### 5. Build cho production

```bash
npm run build
```

Output sẽ nằm trong thư mục `dist/`.

---

## Biến môi trường

### Backend (production)

| Biến | Mô tả |
|---|---|
| `DB_URL` | JDBC URL của database |
| `DB_USER` | Username database |
| `DB_PASS` | Password database |
| `JWT_SECRET` | Secret key để ký JWT |
| `PORT` | Port server lắng nghe (mặc định: `8080`) |

### Frontend

| Biến | Mô tả |
|---|---|
| `VITE_API_URL` | URL của Backend API (mặc định: `http://localhost:8080`) |

---

## Deploy với Docker

### Build image

```bash
cd OptibuildHub_Backend
docker build -t optibuildhub-backend .
```

### Chạy container

```bash
docker run -p 8080:8080 \
  -e DB_URL=jdbc:mysql://<host>:3306/optibuild_hub \
  -e DB_USER=root \
  -e DB_PASS=your_password \
  -e JWT_SECRET=your_jwt_secret \
  optibuildhub-backend
```

---

## Cấu trúc thư mục

```
Optibuild/
├── OptibuildHub_Backend/       # Spring Boot API
│   ├── src/main/java/com/optibuildhub/
│   │   ├── auth/               # Đăng ký, đăng nhập, JWT
│   │   ├── user/               # Quản lý người dùng, phân quyền
│   │   ├── part/               # Linh kiện, lịch sử giá
│   │   ├── pcbuild/            # Xây dựng và kiểm tra cấu hình PC
│   │   ├── forum/              # Bài đăng, bình luận, reaction
│   │   ├── rating/             # Đánh giá linh kiện
│   │   ├── crawler/            # Crawl giá tự động
│   │   ├── file/               # Upload file
│   │   ├── admin/              # Quản trị
│   │   └── config/             # Security, CORS, JWT config
│   └── uploads/                # Thư mục lưu file upload (local)
│
└── OptibuildHub_Frontend/      # React + Vite
    └── src/
        ├── pages/              # Các trang chính
        ├── components/         # UI components dùng chung
        ├── api/                # Axios API calls
        ├── context/            # React Context (Auth)
        └── utils/              # Hằng số, helper functions
```
