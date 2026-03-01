# Student Management System

## Danh sach nhom

| MSSV | Ho va Ten |
|------|-----------|
|      |           |
|      |           |

## Public URL

> (Se cap nhat sau khi deploy len Render)

## Huong dan chay du an

### Yeu cau

- JDK 17+
- Maven (hoac dung Maven Wrapper di kem)
- PostgreSQL (hoac dung Neon.tech)

### Cac buoc chay (Local)

1. Tao file `.env` trong thu muc `student-management/` (tham khao `.env.example`)
2. Chay ung dung:

```bash
cd student-management
./mvnw spring-boot:run
```

Ung dung chay tai: `http://localhost:8080`

### Chay voi Docker

```bash
cd student-management
docker compose up --build
```

### API Endpoints (Lab 2 - REST API)

| Chuc nang        | Method | Endpoint              |
|------------------|--------|-----------------------|
| Lay danh sach    | GET    | /api/students         |
| Lay chi tiet     | GET    | /api/students/{id}    |
| Them sinh vien   | POST   | /api/students         |
| Cap nhat         | PUT    | /api/students/{id}    |
| Xoa sinh vien    | DELETE | /api/students/{id}    |

### Web Pages (Lab 3 & 4 - SSR Thymeleaf)

| Chuc nang                  | Method | URL                              |
|----------------------------|--------|----------------------------------|
| Danh sach sinh vien        | GET    | /students                        |
| Tim kiem theo ten          | GET    | /students?keyword={ten}          |
| Them sinh vien moi         | GET    | /students/new                    |
| Xem chi tiet               | GET    | /students/{id}                   |
| Chinh sua sinh vien        | GET    | /students/{id}/edit              |

## Cau hoi ly thuyet

### Lab 1

**1. Tai sao Database lai chan thao tac Insert khi id bi trung?**

Vi `id` la Primary Key - rang buoc dam bao moi ban ghi trong bang la duy nhat. Neu cho phep trung `id`, he thong khong the phan biet duoc cac ban ghi, dan den xung dot khi truy van, cap nhat hoac xoa du lieu.

**2. Thu Insert sinh vien voi name = NULL. Database co bao loi khong?**

Khong. SQLite cho phep `NULL` vi bang khong co rang buoc `NOT NULL` tren cot `name`. Dieu nay gay ra van de khi code Java doc du lieu len: cac truong `String` co the nhan gia tri `null`, de gay `NullPointerException` neu khong kiem tra truoc khi su dung.

**3. Tai sao moi lan tat ung dung va chay lai, du lieu cu bi mat het?**

Do cau hinh `spring.jpa.hibernate.ddl-auto=create`. Moi lan khoi dong, Hibernate se xoa toan bo bang cu va tao lai bang moi (DROP + CREATE). Doi sang `update` de giu du lieu cu, hoac `none` de Hibernate khong tac dong vao schema.

### Lab 3

**1. @Controller khac gi @RestController?**

`@Controller` tra ve ten View (HTML template) de Thymeleaf render ra trang web hoan chinh. `@RestController` = `@Controller` + `@ResponseBody`, tra ve du lieu tho (JSON/XML) truc tiep cho client. Trong Lab 3, dung `@Controller` de Spring Boot tim file HTML trong `templates/` va render truoc khi tra ve cho trinh duyet.

**2. Model trong Spring MVC dung de lam gi?**

`Model` la doi tuong trung gian truyen du lieu tu Controller sang View (Thymeleaf template). Controller goi `model.addAttribute("key", value)` de dong goi du lieu, sau do Thymeleaf truy cap qua cu phap `${key}` de hien thi du lieu dong trong HTML.

**3. So sanh SSR (Server-Side Rendering) va CSR (Client-Side Rendering)?**

- **SSR**: Server render HTML day du (co du lieu) roi tra ve cho trinh duyet. Uu diem: SEO tot, load lan dau nhanh. Nhuoc diem: moi tuong tac can request lai server.
- **CSR**: Server tra ve HTML rong + JavaScript. Trinh duyet chay JS de goi API va render giao dien. Uu diem: tuong tac muot ma, khong can tai lai trang. Nhuoc diem: SEO kem hon, load lan dau cham hon.

### Lab 4 & 5

**1. Tai sao can chuyen tu SQLite sang PostgreSQL khi deploy?**

SQLite luu du lieu vao file tren disk, phu hop cho dev/prototype. Khi deploy len cloud (Render), filesystem la tam thoi (ephemeral) - moi lan redeploy se mat du lieu. PostgreSQL la database server doc lap, du lieu duoc luu tru ben vung, ho tro nhieu ket noi dong thoi, phu hop cho moi truong production.

**2. Dockerfile multi-stage build co loi gi?**

Multi-stage build tach qua trinh build (Stage 1: dung Maven image ~800MB) va run (Stage 2: chi dung JRE image ~200MB). Image cuoi cung chi chua file .jar va JRE, giam dung luong tu ~1GB xuong ~250MB, tang toc deploy va giam tai nguyen.

**3. Tai sao can dung bien moi truong (.env) thay vi hardcode credentials?**

- **Bao mat**: Khong lo thong tin nhay cam (password, connection string) len Git.
- **Linh hoat**: Cung mot code chay tren nhieu moi truong (dev, staging, production) chi can thay doi bien moi truong.
- **Tuan thu 12-Factor App**: Cau hinh tach biet khoi code la best practice trong phat trien phan mem hien dai.
