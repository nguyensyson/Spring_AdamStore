
# 📌 Dự án Adam Store

## I. Các thông tin trong dự án

### 🔐 Tài khoản Admin mặc định

- Email: `admin@gmail.com`  
- Password: `123456`

### 🎨 Phối màu tham khảo
- [https://colorhunt.co](https://colorhunt.co)

---

## 🧱 Các Entity trong dự án

> Dưới đây là danh sách các thực thể (entities) trong hệ thống và các mối quan hệ giữa chúng. Để dễ quản lý, các mối quan hệ được biểu diễn theo kiểu `(n-1)`, `(1-n)`, `(1-1)`, `(n-n)`.

Chi tiết các entity và trường liên quan, bạn có thể xem phần **Entity Chi tiết** bên dưới hoặc tách riêng file `docs/entities.md`.

---

## ⚙️ Các trạng thái quan trọng

### Trạng thái đơn hàng (`orderStatus`)

| Trạng thái | Ý nghĩa                |
|-----------|------------------------|
| PENDING   | Đang chờ               |
| PROCESSING| Đang xử lý             |
| SHIPPED   | Đã giao hàng           |
| DELIVERED | Đã giao thành công     |
| CANCELLED | Đã huỷ                |

### Trạng thái thanh toán (`paymentStatus`)

| Trạng thái | Ý nghĩa                                                 |
|-----------|----------------------------------------------------------|
| PAID      | Thanh toán thành công                                   |
| PENDING   | Chưa xác nhận thanh toán                                |
| REFUNDED  | Đã hoàn tiền                                             |
| CANCELED  | Đã hủy giao dịch                                        |
| FAILED    | Thanh toán thất bại (lỗi kỹ thuật, từ chối ngân hàng…) |

---

## 🛠️ Các trang quản lý của Admin

- Quản lý Branch, Category, Promotion, Color, Size, Ward, District, Province, Address, Role, Permission
- Quản lý Order, Payment History, Revenue
- Quản lý Product & Product Variant

---

## 🗑️ Xoá dữ liệu

### Xoá mềm (Soft Delete)

> Dữ liệu vẫn còn trong DB, chỉ chuyển status sang `INACTIVE`, có thể khôi phục (API Restore)

Áp dụng cho: User, Category, Branch, Promotion, Product, ProductVariant, Address

### Xoá cứng (Hard Delete)

> Dữ liệu bị xoá hoàn toàn khỏi DB, không thể khôi phục

Áp dụng cho: Review, Order, PaymentHistory, Color, ProductImage

---

## 🧪 API - Hướng dẫn sử dụng

- Tất cả các API `fetch all` có tham số `pageNo`, `pageSize`, `sortBy` (`field-asc`, `field-desc`)
- API tìm kiếm dùng định dạng `field~value`, `field>value`, `field<value`
  - VD: `name~ao`, `price>500000`, `averageRating<4.5`
- API tìm kiếm đơn hàng (cho cả user và admin): sử dụng `search`, hệ thống sẽ tự động lấy theo `token` nếu là user

---

## 👤 Thêm Admin mới

Dùng API `Create User`, truyền vào `RoleIds` tương ứng với Admin. Chỉ Admin mới có quyền gọi API này để thêm các admin khác.

---

## 🔄 Update thông tin

- Với các API `update`, chỉ cần truyền những trường bạn muốn cập nhật (trừ `update product` cần luôn truyền `price` và `quantity`).

---

## 🛒 Quy trình giỏ hàng & đặt hàng

1. **Thêm vào giỏ**:
   - Tìm `ProductVariant` qua `productId`, `size`, `color`
   - Nếu đã có trong giỏ -> cập nhật `quantity`, chưa có -> tạo mới `CartItem`

2. **Đặt hàng**:
   - Gọi API lấy địa chỉ, khuyến mãi, tính phí ship
   - Gọi API tạo Order (status `PENDING`) + PaymentHistory
   - Nếu chọn VNPAY -> tạo URL chuyển hướng, FE gọi callback để cập nhật trạng thái thanh toán

3. **Thanh toán lại**:
   - Tạo `PaymentHistory` mới
   - Nếu thất bại -> Order chuyển sang chờ thanh toán, quá 24h sẽ huỷ tự động

---

## ✍️ Đánh giá sản phẩm

- Sau khi mua hàng thành công, user có thể gọi API để tạo hoặc update đánh giá (review)

---

## 📦 Trạng thái đơn hàng & quản lý doanh thu

- Admin/user gọi `API search Order` để lọc theo trạng thái
- Admin gọi API để lấy doanh thu theo thời gian `startDate`, `endDate`

---

## 🧩 Entity Chi tiết

> Nội dung chi tiết về tất cả entity đã được mô tả trong tài liệu gốc. Bạn có thể tách thành file `docs/entities.md` nếu muốn chia nhỏ README.

---

## ⛳ Gợi ý

Bạn có thể chia README này thành nhiều file nhỏ hơn trong thư mục `docs/` như:

- `docs/entities.md`
- `docs/api-guide.md`
- `docs/order-process.md`
- `docs/admin-panel.md`
