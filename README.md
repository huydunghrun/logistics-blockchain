# 🚢 Hệ thống Quản lý Vận chuyển Hàng hóa Xuất Nhập khẩu ứng dụng Blockchain

Một nền tảng quản lý logistics tích hợp **Consortium Blockchain (Hyperledger Fabric)**, đảm bảo minh bạch, bất biến và tự động hóa toàn bộ chuỗi cung ứng xuất nhập khẩu.

---

## 📌 Chức năng chính

- 📦 Quản lý lô hàng xuất nhập khẩu theo vòng đời đầy đủ (tạo → duyệt → vận chuyển → thông quan → giao hàng).
- 🔗 Ghi nhận trạng thái lô hàng bất biến trên Hyperledger Fabric Ledger.
- 📄 Lưu trữ chứng từ phi tập trung qua IPFS (Invoice, B/L, C/O, Tờ khai hải quan...).
- ✅ Xác minh chứng từ bởi hải quan và ngân hàng.
- 🏦 Smart Contract tự động giải phóng thanh toán khi điều kiện thỏa mãn.
- 📡 Theo dõi lô hàng theo thời gian thực qua WebSocket.
- 🔔 Hệ thống thông báo realtime cho các bên liên quan.
- 👥 Phân quyền theo tổ chức: Nhà xuất khẩu, Hãng vận tải, Hải quan, Ngân hàng.

---

## ⚙️ Công nghệ sử dụng

| Thành phần            | Mô tả                                                  |
|-----------------------|--------------------------------------------------------|
| Hyperledger Fabric    | Consortium Blockchain — ghi nhận giao dịch bất biến   |
| IPFS                  | Lưu trữ chứng từ phi tập trung                        |
| Python / FastAPI      | Backend REST API (async)                              |
| SQLAlchemy (async)    | ORM tương tác PostgreSQL                              |
| PostgreSQL            | Cơ sở dữ liệu quan hệ                                 |
| asyncpg               | Driver async PostgreSQL                               |
| passlib / argon2      | Mã hóa mật khẩu                                       |
| JWT (python-jose)     | Xác thực người dùng                                   |
| WebSocket             | Cập nhật trạng thái realtime                          |
| HTML / CSS / JS       | Giao diện Web frontend                                |

---

## 🚀 Hướng dẫn chạy dự án

### 1. Clone & cài đặt

```bash
git clone https://github.com/your-username/logistics-blockchain
cd logistics-blockchain/backend
python -m venv venv
venv\Scripts\activate        # Windows
# source venv/bin/activate   # Linux/Mac
pip install -r requirements.txt
```

### 2. Cấu hình `.env`

Tạo file `.env` trong thư mục `backend/`:

```env
APP_NAME=Logistics Blockchain API
SECRET_KEY=your-secret-key-here
DATABASE_URL=postgresql://logistics:logistics123@localhost:5432/logistics_db
CORS_ORIGINS=http://localhost:5500,http://127.0.0.1:5500
IPFS_HOST=localhost
IPFS_PORT=5001
UPLOAD_DIR=./uploads
```

### 3. Khởi động PostgreSQL và tạo database

```sql
CREATE USER logistics WITH PASSWORD 'logistics123';
CREATE DATABASE logistics_db OWNER logistics;
```

### 4. Chạy Backend

```bash
uvicorn app.main:app --reload --port 8000
```

API tự động tạo bảng khi khởi động. Truy cập docs tại:
```
http://127.0.0.1:8000/docs
```

### 5. Chạy Frontend

```bash
cd frontend
python -m http.server 5500
```

Mở trình duyệt tại:
```
http://127.0.0.1:5500/pages/login.html
```

---

## 🧪 Tài khoản demo

| Username   | Mật khẩu | Tổ chức                        |
|------------|----------|--------------------------------|
| exporter1  | secret   | 📦 Nhà xuất khẩu — Vinatex    |
| carrier1   | secret   | 🚢 Hãng vận tải — Vina Shipping|
| customs1   | secret   | 🛃 Hải quan — Tổng cục Hải quan|
| bank1      | secret   | 🏦 Ngân hàng — Vietcombank     |

---

## 🧠 Cấu trúc thư mục

```
logistics-blockchain/
├── backend/
│   ├── app/
│   │   ├── api/
│   │   │   ├── auth.py           # Xác thực người dùng
│   │   │   ├── shipments.py      # Quản lý lô hàng
│   │   │   ├── documents.py      # Quản lý chứng từ
│   │   │   └── notifications.py  # Thông báo realtime
│   │   ├── core/
│   │   │   ├── config.py         # Cấu hình hệ thống
│   │   │   ├── database.py       # Kết nối database (async)
│   │   │   └── security.py       # JWT, mã hóa mật khẩu
│   │   ├── models/
│   │   │   ├── db_models.py      # SQLAlchemy models
│   │   │   └── schemas.py        # Pydantic schemas
│   │   ├── services/
│   │   │   ├── user_service.py   # Xử lý logic người dùng
│   │   │   ├── ipfs_service.py   # Tích hợp IPFS
│   │   │   └── fabric_service.py # Tích hợp Hyperledger Fabric
│   │   └── main.py               # Khởi động FastAPI app
│   ├── requirements.txt
│   └── .env
├── frontend/
│   ├── pages/
│   │   ├── login.html
│   │   ├── dashboard.html
│   │   ├── shipments.html
│   │   ├── documents.html
│   │   └── tracking.html
│   ├── js/
│   │   ├── api.js                # API client & Auth
│   │   └── layout.js             # App shell (sidebar, topbar)
│   └── css/
│       └── main.css
└── README.md
```

---

## 📷 Luồng hoạt động

1. Đăng nhập bằng tài khoản tổ chức tương ứng.
2. Nhà xuất khẩu tạo lô hàng mới.
3. Các bên liên quan duyệt và cập nhật trạng thái lô hàng.
4. Tải lên chứng từ — file lưu trên IPFS, hash ghi lên Blockchain.
5. Hải quan / Ngân hàng xác minh chứng từ.
6. Smart Contract tự động giải phóng thanh toán khi đủ điều kiện.
7. Theo dõi toàn bộ lịch sử giao dịch trên Ledger.

---

## 💡 Hướng phát triển

- Tích hợp đầy đủ Hyperledger Fabric chaincode thực tế.
- Hỗ trợ chữ ký số điện tử trên chứng từ.
- Tích hợp thanh toán Letter of Credit (L/C) tự động.
- Xuất báo cáo lịch sử vận chuyển ra file PDF/Excel.
- Hỗ trợ đa ngôn ngữ (EN/VI).
- Mobile app cho theo dõi lô hàng.

---

## 👤 Tác giả

- **[Tên tác giả]** — [Trường/Đơn vị]
- 🎓 [Tên trường đại học]
