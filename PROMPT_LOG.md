# NHẬT KÝ PROMPT ENGINEERING (VIBE CODING LOG)

| Thông tin | Chi tiết |
|-----------|----------|
| **Họ và tên** | Lê Chí Hưng |
| **MSSV** | 125000672 |
| **Tên Website** | NEXUS Gaming Hub — Cửa hàng game trực tuyến |
| **Công cụ AI** | Antigravity (Google DeepMind) — Gemini 2.5 Flash / Claude Sonnet 4.6 |
| **File nộp** | `index.html` · `style.css` · `assets/images/` (36 file) · `PROMPT_LOG.md` |

---

## 1. Yêu cầu ban đầu (System / Initial Prompt)

**Prompt gốc:**
> *"bạn là một coder chuyên nghiệp, hãy code ra cho tôi trang web này như trong hình có đầy đủ chức năng như một website thật thụ, không cần giải thích các dòng code"*

**Kèm theo:** Ảnh chụp giao diện tham khảo — dark gaming UI kiểu Epic Games/Steam, màu nền `#12131c`, điểm nhấn Neon Lime `#b6f800`, sidebar icon, lưới game card.

**AI phản hồi ban đầu:** Tạo `index.html` + `style.css` + `app.js` — cấu trúc cơ bản nhưng chưa đúng chuẩn Semantic HTML5, còn dùng nhiều thẻ `<div>` vô nghĩa.

---

## 2. Quá trình Tinh chỉnh & Hỏi — Đáp với AI

---

### 🔁 Lần 1 — Yêu cầu đúng chuẩn Semantic HTML5

**Prompt:**
> *"bạn là một coder chuyên nghiệp, hãy code ra cho tôi trang web này như trong hình"*

**Tôi chỉ đạo AI:** Phải dùng đúng thẻ ngữ nghĩa HTML5, không lạm dụng `<div>`. Sidebar dùng `<aside>`, điều hướng dùng `<nav>`, nội dung chính dùng `<main>`, từng khu vực dùng `<section>`, từng sản phẩm dùng `<article>`, chân trang dùng `<footer>`.

| Tiêu chí rubric đạt được | Kết quả |
|--------------------------|---------|
| 5+ thẻ Semantic HTML5 | ✅ Dùng 7 thẻ: `header`, `nav`, `main`, `section`, `article`, `aside`, `footer` |
| Không lạm dụng `<div>` | ✅ Mỗi `<div>` có class mô tả rõ chức năng |
| CSS Variables cho màu/font | ✅ Khai báo đầy đủ `--neon-lime`, `--bg-primary`, `--radius-md`… trong `:root` |
| Layout Flexbox/Grid | ✅ Sidebar + main dùng Flexbox; lưới game dùng CSS Grid tự động co giãn |

---

### 🔁 Lần 2 — Loại bỏ file JavaScript, dùng Pure CSS

**Prompt:**
> *"không cần dùng file java"*

**Tôi chỉ đạo AI:** Tôi biết CSS có thể xử lý tương tác qua `:checked`, `:target`, và sibling selector `~` mà không cần JavaScript. Yêu cầu AI xóa `app.js` và áp dụng kỹ thuật này.

| Tính năng | Kỹ thuật Pure CSS được dùng |
|-----------|----------------------------|
| Điều hướng tab (Store / Library / Home…) | `radio input` + `:checked ~` hiển thị view tương ứng |
| Modal chi tiết game | CSS `:target` — click link `href="#modal-id"` mở modal |
| Drawer Giỏ Hàng / Ví / Bạn Bè | `checkbox input` + `label[for]` toggle drawer |
| Filter chip Bán Chạy / Giảm Giá / Miễn Phí | `radio input` + sibling selector ẩn/hiện game card |

**Nhận xét:** Đây là kỹ thuật CSS nâng cao — tôi chủ động chỉ đạo AI áp dụng thay vì để AI dùng JavaScript mặc định.

---

### 🔁 Lần 3 — Tích hợp Animate.css + AOS + Mở rộng nội dung

**Prompt:**
> *"thêm nhiều sản phẩm game vô và tương tác một cách mượt mà có thể dùng được"*

**Tôi chỉ đạo AI:** Dùng đúng 2 thư viện theo rubric — **Animate.css** cho animation tĩnh, **AOS** cho animation khi scroll. Không tạo file JS riêng, phải inline `AOS.init()` trong thẻ `<script>` cuối body.

**Kết quả:**
- **Animate.css 4.1.1**: Logo dùng `bounceIn`, tiêu đề section dùng `fadeInLeft`, badge khuyến mãi dùng `pulse infinite`
- **AOS 2.3.4**: Từng game card dùng `data-aos="fade-up"` với delay tăng dần tạo hiệu ứng lần lượt
- Mở rộng lên **12 game**: Wukong · Elden Ring · RDR2 · BG3 · Tsushima · Forza 5 · Cyberpunk · GTA V · FC 24 · Far Cry 5 · COD MW2 · NFS
- Carousel 3 slide Weekend Deal (điều hướng bằng radio Pure CSS)
- Modal chi tiết đầy đủ (banner, mô tả, tag thể loại, giá, nút mua) cho từng game

**⚠️ Sự cố:** AOS không kích hoạt khi scroll trong `.content-panel` (div nội bộ, không phải `window`)

**Tôi phân tích:** AOS mặc định lắng nghe sự kiện scroll trên `window`, nhưng layout dùng div con có scroll riêng — `window` không nhận được sự kiện.

**Debug prompt:**
> *"AOS không chạy khi cuộn trang"*

**AI sửa:** Thêm event listener lắng nghe scroll trên `.content-panel` và gọi `AOS.refresh()` — inline trong `<script>`, không tạo file .js. AOS hoạt động hoàn toàn sau khi sửa.

---

### 🔁 Lần 4 — Thêm Favicon & Avatar Gamer

**Prompt:**
> *"thêm avatar trang web cho đẹp"*

**Tôi chỉ đạo AI:** Tạo SVG thuần (không dùng ảnh ngoài). Favicon phải liên kết đủ 3 thẻ `<link>` để tương thích mọi trình duyệt.

**Kết quả:**
- `favicon.svg` — Icon tay cầm game viền Neon Lime phát sáng, nền tối bo góc. Hiển thị đúng trên tab trình duyệt.
- `user-gamer-avatar.svg` — Avatar Esports Cyberpunk: tai nghe gaming, kính visor phát sáng, SVG neon glow filter
- Badge cấp bậc **LV.88** (pill Neon Lime, góc dưới avatar)
- Chấm trạng thái online nhấp nháy liên tục bằng `@keyframes`
- Đồng bộ avatar tại: Header · Profile Menu · Danh sách Bạn Bè · Chat

---

### 🔁 Lần 5 — Đổi tên tài khoản người dùng

**Prompt:**
> *"chỉnh tên thành HƯNG"*

**Tôi chỉ đạo AI:** Tìm và thay thế đồng bộ tất cả occurrences trong toàn file, không bỏ sót.

| Vị trí | Kết quả |
|--------|---------|
| Header `.user-name` | ✅ → HƯNG |
| Profile Dropdown | ✅ → HƯNG #1313 |
| Khu vực Cộng Đồng | ✅ → Đăng bởi: HƯNG #1313 |
| Cửa sổ Chat | ✅ → "Ê Hưng, ông mua Black Myth Wukong chưa?" |

---

### 🔁 Lần 6 — Fix lỗi không Thêm Vào Giỏ Hàng được

**Prompt:**
> *"KHÔNG THÊM VÀO GIỎ HÀNG ĐƯỢC CẦN FIX LẠI"*

**Tôi tự phân tích lỗi trước khi gửi prompt:** Nút "Thêm Vào Giỏ Hàng" trong modal là `<label for="cart-toggle">`. Để kích hoạt drawer, nó phải nằm đúng layer — nhưng `.modal-wrapper` có `z-index: 2000` đang chắn `.drawer-backdrop` chỉ có `z-index: 1000`. Sự kiện click bị block bởi xung đột layer.

**AI sửa:** Nâng `z-index` của `.drawer-backdrop` và `.modal-backdrop` lên `3000` — drawer Giỏ Hàng mở mượt mà ngay từ bên trong modal.

---

## 3. Đánh giá cá nhân

**AI tiết kiệm thời gian nhất ở phần nào?**

Xây dựng hệ thống **Pure CSS Interactivity** — các selector phức tạp như `:target`, `:checked ~`, sibling filter mà AI hoàn thành sau 1 prompt. Ngoài ra, tạo 12 modal game có cấu trúc nhất quán cũng tiết kiệm rất nhiều thời gian lặp lại.

**Bài học Prompt Engineering rút ra:**

| # | Bài học | Ví dụ thực tế |
|---|---------|---------------|
| 1 | **Cụ thể & kỹ thuật** | "thêm `@keyframes` glow neon khi hover" thay vì "làm đẹp hơn" |
| 2 | **Nêu ràng buộc từ đầu** | Nói "không dùng file JS" ngay từ đầu → tránh phải refactor |
| 3 | **Tự phân tích trước khi debug** | Hiểu z-index conflict → prompt chính xác hơn, AI sửa đúng ngay |
| 4 | **Mô tả triệu chứng, không mô tả giải pháp** | "click không mở drawer" → AI tự tìm root cause hiệu quả hơn |
| 5 | **Luôn test trên trình duyệt thật** | Lỗi AOS scroll & z-index chỉ phát hiện khi chạy thực tế |
