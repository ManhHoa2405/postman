# 📊 BÁO CÁO KIỂM THỬ API VÀ ĐÁNH GIÁ HIỆU NĂNG HỆ THỐNG

**Tên Dự Án:** API Testing & Performance Evaluation (E-commerce Module)  
**Ngày Kiểm Thử:** 17/06/2026  
**Người Kiểm Thử:** Nguyễn Mạnh Hòa  

---

## 🎯 1. Mục Tiêu Kiểm Thử
*   **Kiểm thử chức năng (Functional Testing):** Đánh giá tính chính xác của các API endpoints quản lý sản phẩm thông qua Postman, kiểm tra cấu trúc dữ liệu JSON trả về và các mã trạng thái HTTP.
*   **Kiểm thử chịu tải (Load Testing):** Sử dụng Apache JMeter để giả lập hàng trăm đến hàng ngàn người dùng truy cập cùng lúc, từ đó phân tích khả năng đáp ứng và giới hạn chịu đựng của máy chủ.

## 💻 2. Môi Trường & Công Cụ Kiểm Thử
*   **Phần mềm kiểm thử API:** Postman Desktop App.
*   **Phần mềm kiểm thử tải:** Apache JMeter.
*   **Hệ điều hành:** Windows / macOS.
*   **Các Endpoint API được sử dụng để giả lập:**
    *   Hệ thống E-commerce: `https://fakestoreapi.com/`
    *   Hệ thống User Data: `https://reqres.in/`

## ⚙️ 3. Phương Pháp Thực Hiện
*   **Kiểm thử thủ công:** Thiết lập các thông số (Headers, Query, Body) trực tiếp trên giao diện Postman để gửi request.
*   **Kiểm thử tự động:** Tích hợp các đoạn mã JavaScript vào tab **Tests** của Postman để hệ thống tự động đối chiếu kết quả (Assertion).
*   **Kiểm thử hiệu năng:** Cấu hình Thread Group trong JMeter với kịch bản 100 và 1000 Virtual Users (VU), theo dõi qua các báo cáo View Results Tree và Summary Report.

---

## 🧪 4. Chi Tiết Kịch Bản Kiểm Thử Chức Năng (Postman)

### Kịch Bản 1: Lấy danh sách sản phẩm với tham số giới hạn
*   **Mục Đích:** Kiểm tra khả năng lọc dữ liệu của API khi truyền tham số giới hạn số lượng bản ghi trả về.
*   **Phương Thức HTTP:** `GET`
*   **URL:** `https://fakestoreapi.com/products`
*   **Tham Số (Query):** `limit=2`
*   **Kết Quả Mong Đợi:** API trả về mảng dữ liệu chứa đúng 2 sản phẩm đầu tiên, mã trạng thái `200 OK`.
*   **Trạng Thái:** ✅ **Thành công (PASSED)**
*   **Dữ liệu thực tế trả về (Response Body):**

```json
[
  {
    "id": 1,
    "title": "Fjallraven - Foldsack No. 1 Backpack, Fits 15 Laptops",
    "price": 109.95,
    "description": "Your perfect pack for everyday use and walks in the forest.",
    "category": "men's clothing",
    "image": "[https://fakestoreapi.com/img/81fPKd-2AYL._AC_SL1500_.jpg](https://fakestoreapi.com/img/81fPKd-2AYL._AC_SL1500_.jpg)",
    "rating": { "rate": 3.9, "count": 120 }
  },
  {
    "id": 2,
    "title": "Mens Casual Premium Slim Fit T-Shirts ",
    "price": 22.3,
    "description": "Slim-fitting style, contrast raglan sleeve, three-button henley placket.",
    "category": "men's clothing",
    "image": "[https://fakestoreapi.com/img/71-3HjGNDUL._AC_SY879._SX._UX._SY._UY_.jpg](https://fakestoreapi.com/img/71-3HjGNDUL._AC_SY879._SX._UX._SY._UY_.jpg)",
    "rating": { "rate": 4.1, "count": 259 }
  }
]
<img width="1567" height="1001" alt="image" src="https://github.com/user-attachments/assets/8105eb48-66e8-48fd-923e-86ad037005ea" />
<img width="1567" height="1010" alt="image" src="https://github.com/user-attachments/assets/bb6b01c3-c606-4703-8070-f2d218ac0103" />

Kịch Bản 2: Kiểm tra bắt lỗi khi truy cập URL không tồn tạiMục Đích: Xác thực cơ chế xử lý ngoại lệ của server khi Client gọi sai tên endpoint.Phương Thức HTTP: GETURL: https://fakestoreapi.com/productssss (Cố tình viết sai lỗi chính tả chữ products)Kết Quả Mong Đợi: Server từ chối kết nối và trả về mã lỗi 404 Not Found kèm thông báo không tìm thấy tài nguyên.Trạng Thái: ❌ Thất bại có chủ đích (Phù hợp với thiết kế an toàn)Dữ liệu thực tế trả về: Mã trạng thái 404 Not Found kèm cấu trúc HTML báo lỗi thay vì JSON.
<img width="1542" height="957" alt="image" src="https://github.com/user-attachments/assets/9ecc29bf-1893-448e-911a-3fd54f6c8da6" />

Kịch Bản 3: Thêm mới một sản phẩm vào hệ thốngMục Đích: Kiểm tra phương thức tạo dữ liệu mới, đảm bảo body request được server tiếp nhận và xử lý.Phương Thức HTTP: POSTURL: https://fakestoreapi.com/productsBody Truyền Vào (JSON):JSON{
    "title": "Áo thun nam phong cách mới",
    "price": 15.5,
    "description": "Áo thun chất liệu cotton thoáng mát",
    "category": "men's clothing",
    "image": "[https://i.pravatar.cc](https://i.pravatar.cc)"
}
Kết Quả Mong Đợi: Mã trạng thái 200 OK (hoặc 201 Created), trả về ID của sản phẩm vừa được tạo.Trạng Thái: ✅ Thành công (PASSED)
<img width="1570" height="1011" alt="image" src="https://github.com/user-attachments/assets/ed81f4c4-c371-4e98-b152-c7e82784054b" />

5. Tóm Tắt Kết Quả Kiểm Thử APITổng số kịch bản đã thực hiện: 3Tỷ lệ truy xuất dữ liệu đúng (Pass): 2 kịch bảnTỷ lệ kiểm thử ngoại lệ (Fail - Bắt lỗi 404): 1 kịch bảnKết luận: Các API Endpoint của FakeStore hoạt động ổn định, đáp ứng chuẩn RESTful. Tuy nhiên, phần xử lý lỗi URL (Kịch bản 2) trả về giao diện HTML thô thay vì một JSON Error Message chuẩn hóa.
