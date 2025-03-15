# Cutting Stock Environment

## Giới thiệu

Môi trường này mô phỏng bài toán **Cắt ván gỗ (Cutting Stock Problem)** bằng cách chia nhỏ tấm ván gỗ lớn thành các tấm nhỏ hơn theo kích thước yêu cầu. 
Môi trường được xây dựng trên thư viện **Gymnasium** và sử dụng **Pygame** để trực quan hóa quá trình cắt.

---

## Cấu hình tham số

- **`render_mode`**: Chế độ hiển thị (`human` - hiển thị trực tiếp, `rgb_array` - trả về hình ảnh)
- **`min_w`, `min_h`**: Kích thước tối thiểu của ván gỗ (chiều rộng & chiều cao)
- **`max_w`, `max_h`**: Kích thước tối đa của ván gỗ (chiều rộng & chiều cao)
- **`num_stocks`**: Số lượng ván gỗ cần cắt
- **`max_product_type`**: Số loại sản phẩm tối đa có thể cắt từ ván gỗ
- **`max_product_per_type`**: Số lượng tối đa của mỗi loại sản phẩm
- **`seed`**: Hạt giống ngẫu nhiên để đảm bảo kết quả có thể tái tạo
- **`custom_products`**: Danh sách các sản phẩm tùy chỉnh (nếu có)
- **`custom_stocks`**: Danh sách các ván gỗ tùy chỉnh (nếu có)

---

## Cấu trúc dữ liệu

### Ván gỗ (`_stocks`)
Mảng 3D lưu trạng thái của các ván gỗ, giá trị có thể là:

- `-2`: Vùng không sử dụng (ngoài phạm vi ván gỗ)
- `-1`: Khu vực có thể cắt
- `0, 1, 2, ...`: Các vùng đã được cắt cho sản phẩm tương ứng

### Sản phẩm (`_products`)
Dictionary chứa thông tin về sản phẩm cần cắt:

- **`size`**: Mảng chứa kích thước của từng loại sản phẩm (`(chiều rộng, chiều cao)`).
- **`quantity`**: Số lượng của từng loại sản phẩm cần cắt.

---

## Quy tắc hoạt động

### 1. Khởi tạo môi trường (`reset`)
- Nếu có `custom_stocks` hoặc `custom_products`, sử dụng dữ liệu đầu vào.
- Nếu không, tạo ngẫu nhiên danh sách ván gỗ và sản phẩm cần cắt.
- Trả về trạng thái ban đầu của môi trường.

### 2. Thực hiện hành động (`step`)
- Nhận vào một hành động gồm:
  - **`stock_idx`**: Chọn tấm ván gỗ để cắt
  - **`size`**: Kích thước sản phẩm cần cắt
  - **`position`**: Vị trí đặt sản phẩm trên ván gỗ
- Kiểm tra điều kiện hợp lệ:
  - Ván gỗ tồn tại và đủ kích thước
  - Vị trí cắt hợp lệ, không bị trùng lắp
  - Sản phẩm còn số lượng để cắt
- Nếu hợp lệ, đánh dấu vùng đã cắt trên ván gỗ.
- Kiểm tra xem tất cả sản phẩm đã được cắt xong hay chưa (`done`).
- **Tính toán phần thưởng (`reward`)**: `- (Tổng số lượng sản phẩm chưa cắt)` (càng ít càng tốt).

### 3. Hiển thị môi trường (`_render_frame`)
- Dùng **Pygame** để vẽ ván gỗ và sản phẩm đã cắt.
- Vẽ lưới để dễ quan sát kích thước ván gỗ.
- Cập nhật cửa sổ hiển thị hoặc trả về mảng ảnh.

### 4. Đóng môi trường (`close`)
- Tắt cửa sổ hiển thị và giải phóng tài nguyên.

---
