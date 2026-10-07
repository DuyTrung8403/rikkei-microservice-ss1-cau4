### 1. User & Identity Service (Quản lý Tài khoản & Định danh)
* **Trách nhiệm đảm nhận:** Là "người gác cổng" của Shopee. Quản lý toàn bộ quy trình đăng ký, đăng nhập (Authentication) và phân quyền (Authorization). Nó lưu trữ hồ sơ cá nhân của Người mua (Buyer) và Người bán (Seller), quản lý danh bạ địa chỉ giao hàng, và duy trì phiên đăng nhập của người dùng.
* **Đặc thù:** Cần chịu tải lượng truy cập khổng lồ vì bất kỳ hành động nào trên app cũng phải đi qua lớp xác thực này.

### 2. Product Catalog Service (Quản lý Danh mục Sản phẩm)
* **Trách nhiệm đảm nhận:** Đóng vai trò là quyển "catalogue" khổng lồ. Lưu trữ và phân phối mọi thông tin liên quan đến mặt hàng (Tên, mô tả, hình ảnh, video, phân loại hàng, thuộc tính màu sắc/kích cỡ). Cung cấp giao diện cho Seller đăng bán sản phẩm và phân phối dữ liệu cho Buyer xem hàng.
* **Đặc thù:** Tỷ lệ Đọc/Ghi (Read/Write ratio) cực kỳ chênh lệch (Buyer xem hàng nhiều gấp hàng nghìn lần Seller cập nhật hàng). Đòi hỏi cơ chế Caching (bộ nhớ đệm) rất mạnh.

### 3. Order Management Service / OMS (Quản lý Đơn hàng)
* **Trách nhiệm đảm nhận:** "Trái tim" của hệ thống E-commerce. Module này quản lý vòng đời hoàn chỉnh của một đơn hàng. Bắt đầu từ lúc gom hàng trong Giỏ (Cart), nhấn Thanh toán (Checkout), tính toán tổng tiền, cho đến khi luân chuyển trạng thái đơn hàng (Chờ xác nhận ➡️ Đang giao ➡️ Hoàn thành / Hủy bỏ).
* **Đặc thù:** Nghiệp vụ phức tạp nhất, đóng vai trò điều phối (Orchestrator), phải gọi chéo sang rất nhiều module khác (Payment, Shipping, Inventory) để hoàn thành một đơn.

### 4. Payment & Escrow Service (Thanh toán & Giao dịch đảm bảo)
* **Trách nhiệm đảm nhận:** Xử lý tất cả các luồng tiền tệ. Tích hợp cổng thanh toán bên thứ ba (VNPay, Thẻ tín dụng) và ví nội bộ (ShopeePay). Đặc biệt, thực hiện nghiệp vụ **Escrow (Giao dịch đảm bảo)** — giữ tiền của người mua và chỉ giải ngân (nhả tiền) cho người bán khi người mua xác nhận đã nhận hàng thành công. Xử lý cả luồng Hoàn tiền (Refund).
* **Đặc thù:** Yêu cầu bảo mật cấp độ cao nhất và tính chính xác tuyệt đối của dữ liệu (ACID transactions), không cho phép sai số một đồng nào.

### 5. Logistics & Shipping Service (Vận chuyển & Giao nhận)
* **Trách nhiệm đảm nhận:** Tính toán phí vận chuyển tự động dựa trên khoảng cách địa lý và kích thước bưu kiện. Tích hợp API kết nối với các đối tác vận chuyển (SPX Express, GHTK, J&T NinjaVan...). Nhận dữ liệu hành trình bưu cục và cung cấp thông tin theo dõi đơn hàng (Real-time Tracking) cho người dùng.
* **Đặc thù:** Vì phải kết nối với bên thứ ba (thường hay chậm hoặc rớt mạng), module này phải được thiết kế độc lập để không làm treo toàn bộ app Shopee khi một đối tác giao hàng bị lỗi.

### 6. Promotion & Marketing Service (Quản lý Khuyến mãi)
* **Trách nhiệm đảm nhận:** Cỗ máy thu hút khách hàng của Shopee. Xử lý toàn bộ logic phức tạp về Mã giảm giá (Vouchers), Voucher Freeship, chiến dịch Flash Sale, và hệ thống trừ/tích lũy Shopee Xu. Module này sẽ kiểm tra xem một đơn hàng có thỏa mãn điều kiện áp dụng mã hay không.
* **Đặc thù:** Luật kinh doanh thay đổi liên tục hàng ngày. Phải chịu tải được các đợt "Traffic Spike" (lượng truy cập bùng nổ) khủng khiếp vào các khung giờ vàng săn sale (Flash Sale, 11/11, 12/12).
