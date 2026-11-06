# Thiết Kế Kỹ Thuật: Trang Quản Trị Sinh Viên (Rikkei Student Admin)

## 1. Tổng quan yêu cầu
Dự án xây dựng trang web quản trị sinh viên Rikkei Student Admin khớp với mẫu thiết kế ở hai kích thước tiêu chuẩn 1280px (Desktop) và 360px (Mobile) với dung sai không quá 2px (hoặc 0.125rem).

Các tiêu chí ràng buộc:
- Ngữ nghĩa HTML5 chuẩn, phân chia rõ ràng các vùng (topbar, sidebar, main, footer).
- Duy nhất một tiêu đề `<h1>Tổng quan</h1>` trong vùng nội dung chính.
- Danh sách sinh viên dùng thẻ danh sách (`<ul>` / `<li>`), không dùng `<table>`.
- Form nhập liệu có `<label>` gắn với từng ô qua thuộc tính `for` và `id`. Các trường Họ tên, Mã SV, Email có thuộc tính `required` và `type` chuẩn, tự động kích hoạt HTML5 native validation chặn submit khi để trống.
- Thanh trên cố định (`position: sticky; top: 0; z-index: 100`).
- Responsive:
  - Dưới 768px (bao gồm 360px và 320px): Ẩn thanh menu bên (sidebar) và lời chào + avatar người dùng; hiển thị nút menu ba gạch ở góc phải thanh trên.
  - Từ 768px trở lên: Hiển thị đầy đủ menu bên và lời chào người dùng; ẩn nút menu ba gạch.
  - Không xuất hiện thanh cuộn ngang khi thu màn hình về 320px.
- CSS nằm trong tệp riêng (`style.css`), toàn bộ màu sắc và khoảng cách được định nghĩa qua biến CSS (`:root`).
- Hiệu ứng phản hồi thị giác: Tất cả nút, liên kết và ô nhập có trạng thái `:hover` và `:focus-visible` (qua Tab phím) mà không gây dịch chuyển vị trí hay biến dạng kích thước (dùng outline/box-shadow).
- Chú thích tiếng Việt đầy đủ cho từng khối CSS chính và từng vùng HTML chính.
- Khả năng co giãn: Khi thêm sinh viên thứ 6 hoặc đổi tên thành tên dài hơn, các cột trong danh sách sinh viên ở 1280px vẫn thẳng hàng đều đặn.

---

## 2. Kiến trúc & Cấu trúc Tệp tin

```
Stage-4-BTVN-6/
├── index.html        # Trang giao diện chính với HTML5 semantic
├── style.css         # Toàn bộ mã định dạng, CSS variables, responsive
└── README.md         # (Tùy chọn) Hướng dẫn và thông tin dự án
```

---

## 3. Chi tiết Cấu trúc HTML & Ngữ nghĩa

### 3.1. Thanh trên (`<header class="topbar">`)
- Chú thích: `<!-- ================= THANH TRÊN ================= -->`
- Logo / Brand: `<div class="topbar-brand">Rikkei Student Admin</div>`
- Khối thông tin bên phải:
  - `<div class="topbar-user">` (hiển thị khi $\ge 768\text{px}$):
    - Lời chào: `<span class="user-greeting">Chào, Chị Lan</span>`
    - Avatar: `<div class="user-avatar" aria-label="Avatar Chị Lan">L</div>`
  - Nút menu ba gạch: `<button class="topbar-menu-btn" type="button" aria-label="Mở menu">` (hiển thị khi $< 768\text{px}$)
    - Chứa 3 đường vạch gạch ngang SVG hoặc span bars.

### 3.2. Bố cục tổng thể (`<div class="layout-container">`)
- Bao quanh Sidebar và Main Content.

### 3.3. Menu bên (`<aside class="sidebar">` hoặc `<nav class="sidebar">`)
- Chú thích: `<!-- ================= MENU BÊN ================= -->`
- Thẻ `<nav aria-label="Điều hướng chính">`
- Thẻ danh sách `<ul class="sidebar-menu">`:
  - Item 1: `<li class="sidebar-item active"><a href="#tong-quan" class="sidebar-link">Tổng quan</a></li>`
  - Item 2: `<li class="sidebar-item"><a href="#sinh-vien" class="sidebar-link">Sinh viên</a></li>`
  - Item 3: `<li class="sidebar-item"><a href="#lop-hoc" class="sidebar-link">Lớp học</a></li>`
  - Item 4: `<li class="sidebar-item"><a href="#diem-so" class="sidebar-link">Điểm số</a></li>`
  - Item 5: `<li class="sidebar-item"><a href="#cai-dat" class="sidebar-link">Cài đặt</a></li>`

### 3.4. Vùng nội dung chính (`<main class="main-content">`)
- Chú thích: `<!-- ================= VÙNG NỘI DUNG CHÍNH ================= -->`
- Tiêu đề cấp 1: `<h1 class="page-title">Tổng quan</h1>`
- **Section 1: Thẻ thống kê** (`<section class="stats-section" aria-labelledby="stats-heading">`):
  - Chú thích: `<!-- KHỐI THỐNG KÊ -->`
  - `<h2 id="stats-heading" class="visually-hidden">Thống kê tổng quan</h2>` (đảm bảo cấu trúc heading chuẩn)
  - Lưới 4 thẻ thống kê:
    1. Tổng sinh viên: giá trị `1.250`, biến động `+32 trong tháng`, liên kết `Xem danh sách`, dải màu tím `#6c2bd9`.
    2. Đang học: giá trị `1.080`, biến động `86% tổng số`, liên kết `Xem danh sách`, dải màu xanh `#059669`.
    3. Bảo lưu: giá trị `96`, biến động `+5 trong tháng`, liên kết `Xem danh sách`, dải màu vàng hổ phách `#b45309`.
    4. Nghỉ học: giá trị `74`, biến động `-3 trong tháng`, liên kết `Xem danh sách`, dải màu đỏ `#b91c1c`.
- **Section 2: Danh sách sinh viên** (`<section class="students-section" aria-labelledby="students-heading">`):
  - Chú thích: `<!-- KHỐI DANH SÁCH SINH VIÊN -->`
  - Tiêu đề cấp 2: `<h2 id="students-heading" class="section-title">Sinh viên mới nhập học</h2>`
  - Khối danh sách `<ul class="student-list">` với các item `<li>`:
    - Cột 1: `<div class="student-avatar">A</div>`
    - Cột 2: `<div class="student-info"><span class="student-name">Nguyễn Văn An</span><span class="student-sub">SV2024001 - CNTT01</span></div>`
    - Cột 3: `<span class="student-email">an.nv@rikkei.example</span>` (ẩn trên mobile $< 768\text{px}$)
    - Cột 4: `<span class="student-status status-studying">Đang học</span>` (các trạng thái: Đang học, Bảo lưu, Nghỉ học)
- **Section 3: Form thêm nhanh sinh viên** (`<section class="add-student-section" aria-labelledby="form-heading">`):
  - Chú thích: `<!-- KHỐI FORM THÊM SINH VIÊN -->`
  - Tiêu đề cấp 2: `<h2 id="form-heading" class="section-title">Thêm nhanh sinh viên</h2>`
  - Form `<form class="student-form" action="#" method="post" onsubmit="event.preventDefault();">`:
    - Nhóm Họ tên: `<label for="fullname">Họ tên</label>`, `<input type="text" id="fullname" name="fullname" placeholder="Nguyễn Văn A" required>`
    - Nhóm Mã SV: `<label for="student-id">Mã SV</label>`, `<input type="text" id="student-id" name="student-id" placeholder="SV2024006" required>`
    - Nhóm Lớp: `<label for="class-name">Lớp</label>`, `<select id="class-name" name="class-name"><option value="CNTT01" selected>CNTT01</option>...</select>`
    - Nhóm Email: `<label for="email">Email</label>`, `<input type="email" id="email" name="email" placeholder="ban@rikkei.example" required>`
    - Nút submit: `<button type="submit" class="btn-submit">Thêm sinh viên</button>`

### 3.5. Chân trang (`<footer class="footer">`)
- Chú thích: `<!-- ================= CHÂN TRANG ================= -->`
- Nội dung: `<p class="footer-text">Rikkei Student Admin - Dữ liệu minh họa</p>`

---

## 4. Hệ Thống Biến CSS (`:root`)

```css
:root {
  /* Bảng màu thương hiệu */
  --color-topbar-bg: #22094b;
  --color-sidebar-bg: #22094b;
  --color-sidebar-active: #6c2bd9;
  --color-sidebar-hover: rgba(255, 255, 255, 0.08);
  --color-sidebar-text: #ffffff;
  --color-sidebar-text-muted: rgba(255, 255, 255, 0.7);

  /* Bảng màu trang & card */
  --color-bg-page: #f8f9fa;
  --color-bg-card: #ffffff;
  --color-border: #e5e7eb;
  --color-border-light: #f3f4f6;

  /* Màu chữ */
  --color-text-main: #111827;
  --color-text-muted: #6b7280;
  --color-text-white: #ffffff;

  /* Màu chủ đạo nút & liên kết */
  --color-primary: #6c2bd9;
  --color-primary-hover: #5b21b6;
  --color-primary-light: #ede9fe;

  /* Màu trạng thái & chỉ số */
  --color-accent-purple: #6c2bd9;
  --color-accent-green: #059669;
  --color-accent-amber: #b45309;
  --color-accent-red: #b91c1c;

  --color-badge-green-bg: #dcfce7;
  --color-badge-green-text: #15803d;
  --color-badge-amber-bg: #fef3c7;
  --color-badge-amber-text: #b45309;
  --color-badge-red-bg: #fee2e2;
  --color-badge-red-text: #b91c1c;

  /* Khoảng cách (Spacing) */
  --space-xs: 4px;
  --space-sm: 8px;
  --space-md: 16px;
  --space-lg: 20px;
  --space-xl: 24px;
  --space-2xl: 32px;

  /* Bo góc (Border radius) */
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 12px;
  --radius-full: 9999px;

  /* Đổ bóng */
  --shadow-card: 0 1px 3px rgba(0, 0, 0, 0.05);
  --focus-ring: 0 0 0 3px rgba(108, 43, 217, 0.35);
}
```

---

## 5. Quy chuẩn Thiết kế Đáp ứng (Responsive Breakpoints)

1. **Desktop ($\ge 1024\text{px}$, chuẩn 1280px)**:
   - Layout chia 2 cột: Sidebar rộng 200px cố định bên trái, Main Content chiếm phần còn lại với `max-width: 1040px` hoặc tự co giãn trong khung 1280px.
   - Thống kê: 4 cột (`grid-template-columns: repeat(4, 1fr)`).
   - Danh sách sinh viên: Hiển thị đầy đủ 4 cột (Avatar, Thông tin, Email, Trạng thái) sử dụng `display: grid; grid-template-columns: 40px minmax(170px, 1.2fr) minmax(200px, 1.5fr) auto;`. Đảm bảo các dòng luôn thẳng cột bất kể tên dài hay ngắn.
   - Form: Grid 2 cột cho các trường nhập, nút submit căn trái với kích thước tự nhiên.
2. **Tablet ($768\text{px} - 1023\text{px}$)**:
   - Sidebar vẫn hiển thị. Thống kê 2 cột.
3. **Mobile ($< 768\text{px}$, chuẩn 360px & 320px)**:
   - Sidebar ẩn hoàn toàn (`display: none`).
   - Lời chào và avatar trên thanh topbar ẩn; nút menu 3 gạch xuất hiện ở góc phải.
   - Thống kê: Grid 2 cột x 2 hàng như ảnh 2.
   - Danh sách sinh viên: Ẩn cột email (`display: none`). Cột thông tin chiếm trọn khoảng trống giữa avatar và badge trạng thái.
   - Form: Các trường xếp chồng thành 1 cột dọc (`grid-template-columns: 1fr`). Nút submit mở rộng toàn bộ bề ngang (`width: 100%`).
   - Kiểm tra kỹ ở 320px: tất cả phần tử dùng `box-sizing: border-box`, không có phần tử nào cố định chiều rộng lớn hơn 320px gây tràn trang (`overflow-x`).

---

## 6. Tiêu chuẩn Tương tác & Trợ năng (Accessibility)
- Trạng thái `:hover`:
  - Nút bấm đổi sang màu đậm hơn nhẹ nhàng (`transition: background-color 0.2s ease`).
  - Liên kết gạch chân hoặc đổi màu nhẹ.
  - Các mục menu đổi màu nền tinh tế.
- Trạng thái `:focus-visible` (khi dùng phím Tab):
  - Áp dụng `outline: 2px solid var(--color-primary); outline-offset: 2px;` hoặc `box-shadow: var(--focus-ring);`.
  - Không làm thay đổi kích thước `width`, `height`, `padding`, `border-width` để tránh giật giật / xô lệch bố cục.

