# Rikkei Student Admin Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Xây dựng trang web Rikkei Student Admin hoàn chỉnh gồm `index.html` và `style.css` khớp chuẩn ảnh mẫu ở 1280px và 360px, đáp ứng đầy đủ 9 tiêu chí kỹ thuật và thẩm mỹ.

**Architecture:** Sử dụng kiến trúc HTML5 Semantic kết hợp CSS Grid và Flexbox hiện đại, hệ thống CSS Variables (`:root`) quản lý toàn bộ màu sắc và khoảng cách, thiết kế mobile-first hoặc desktop-down với media query mốc 768px. Đảm bảo cấu trúc danh sách `<ul>/<li>` thẳng cột tuyệt đối bằng CSS Grid và form validation native của HTML5.

**Tech Stack:** HTML5 Semantic, CSS3 (CSS Variables, Flexbox, CSS Grid), SVG icons, Vanilla Web Standards.

**Spec:** [`docs/superpowers/specs/2026-11-06-rikkei-student-admin-design.md`](file:///c:/Users/PC/Desktop/AI%20Driven/Stage-4-BTVN-6/docs/superpowers/specs/2026-11-06-rikkei-student-admin-design.md)

## Global Constraints

- Sai lệch kích thước và vị trí so với ảnh mẫu $\le 2\text{px}$ hoặc $0.125\text{rem}$; màu sắc đúng mã màu thiết kế.
- Cấu trúc: Thanh trên sticky (`position: sticky`), menu bên, main chia thành các section rõ ràng, chân trang căn giữa.
- Duy nhất một tiêu đề `<h1>` (`<h1>Tổng quan</h1>`), thứ bậc `<h2>` hợp lý cho từng section.
- Danh sách sinh viên dùng `<ul>` / `<li>`, không dùng thẻ `<table>`.
- Form dùng đúng loại ô nhập (`text`, `select`, `email`), mỗi ô có `<label for="...">` gắn với `id`. Các trường Họ tên, Mã SV, Email có `required`. Bấm "Thêm sinh viên" khi rỗng sẽ bị trình duyệt chặn.
- Dưới 768px (gồm 360px và 320px): Ẩn menu bên và lời chào/avatar; góc phải thanh trên có biểu tượng menu 3 gạch. Từ 768px trở lên: Hiển thị đầy đủ menu bên và lời chào; ẩn nút 3 gạch.
- Toàn bộ chữ nguyên văn tiếng Việt có dấu. CSS nằm riêng trong `style.css`, màu và khoảng cách dùng CSS Variables.
- Liên kết (kể cả "Xem danh sách"), nút (kể cả nút 3 gạch), ô nhập có hiệu ứng `:hover` và `:focus-visible` (qua Tab) mà không làm đổi kích thước hay xô lệch bố cục.
- Mỗi khối CSS chính (thanh trên, menu bên, thống kê, danh sách sinh viên, form, chân trang) có một dòng chú thích tiếng Việt ngắn; HTML có chú thích đánh dấu các vùng chính.
- Màn hình co về 320px không bị thanh cuộn ngang (`overflow-x`).
- Thêm sinh viên thứ 6 và đổi tên dài hơn không làm vỡ bố cục, các dòng sinh viên vẫn thẳng cột ở 1280px.

## Review Focus

- Trường hợp 1: Nhấn Tab liên tục duyệt qua tất cả link, nút, ô nhập — viền focus hiển thị rõ nét bằng `outline`/`box-shadow`, phần tử không bị co giật hay lệch layout.
- Trường hợp 2: Bấm nút "Thêm sinh viên" khi chưa điền thông tin — trình duyệt hiển thị tooltip nhắc nhở điền Họ tên (hoặc Mã SV/Email), không submit form.
- Trường hợp 3: Nhập sai định dạng email (ví dụ: `abc`) — trình duyệt báo lỗi định dạng email.
- Trường hợp 4: Thu nhỏ cửa sổ trình duyệt xuống 320px — kiểm tra `document.documentElement.scrollWidth <= document.documentElement.clientWidth`, không xuất hiện thanh cuộn ngang.
- Trường hợp 5: Thêm sinh viên thứ 6 với họ tên rất dài "Nguyễn Hoàng Triệu Nhật Minh Khang" — avatar, tên, email và badge vẫn thẳng hàng thẳng cột với 5 sinh viên trước ở màn hình 1280px.

---

### Task 1: Xây dựng cấu trúc HTML5 ngữ nghĩa (`index.html`)

**Files:**
- Create: `c:/Users/PC/Desktop/AI Driven/Stage-4-BTVN-6/index.html`

**Interfaces:**
- Consumes: Cấu trúc từ tài liệu spec `docs/superpowers/specs/2026-11-06-rikkei-student-admin-design.md`
- Produces: File `index.html` chứa đầy đủ markup HTML5, các chú thích tiếng Việt cho các vùng chính, liên kết với `style.css`.

- [ ] **Step 1: Viết thẻ khai báo HTML và header trang**
  - Chú thích: `<!-- ================= THANH TRÊN ================= -->`
  - Thẻ `<header class="topbar">` chứa brand name `Rikkei Student Admin`, khối user greeting `Chào, Chị Lan` + avatar chữ `L`, và nút menu ba gạch `<button class="topbar-menu-btn" type="button" aria-label="Menu">`.

- [ ] **Step 2: Viết khối menu bên (Sidebar)**
  - Chú thích: `<!-- ================= MENU BÊN ================= -->`
  - Thẻ `<aside class="sidebar">` bọc `<nav aria-label="Menu chính">` với danh sách `<ul>` gồm 5 mục: Tổng quan (active), Sinh viên, Lớp học, Điểm số, Cài đặt.

- [ ] **Step 3: Viết khối nội dung chính (Main Content)**
  - Chú thích: `<!-- ================= VÙNG NỘI DUNG CHÍNH ================= -->`
  - Thẻ `<h1>Tổng quan</h1>`
  - Section 1 (Thống kê) có chú thích `<!-- KHỐI THỐNG KÊ -->`, 4 thẻ:
    - Tổng sinh viên: 1.250 | +32 trong tháng | Xem danh sách
    - Đang học: 1.080 | 86% tổng số | Xem danh sách
    - Bảo lưu: 96 | +5 trong tháng | Xem danh sách
    - Nghỉ học: 74 | -3 trong tháng | Xem danh sách
  - Section 2 (Danh sách sinh viên mới nhập học) có chú thích `<!-- KHỐI DANH SÁCH SINH VIÊN -->`, thẻ `<h2>Sinh viên mới nhập học</h2>`, danh sách `<ul>` với 5 sinh viên mẫu (Nguyễn Văn An, Trần Thu Hà, Lê Minh Quân, Phạm Ngọc Mai, Đỗ Quốc Bảo).
  - Section 3 (Form thêm nhanh sinh viên) có chú thích `<!-- KHỐI FORM THÊM SINH VIÊN -->`, thẻ `<h2>Thêm nhanh sinh viên</h2>`, thẻ `<form>` với các trường:
    - Họ tên: `id="fullname"`, `type="text"`, `required`
    - Mã SV: `id="student-id"`, `type="text"`, `required`
    - Lớp: `id="class-name"`, `<select>` (CNTT01, KT02, CNTT03)
    - Email: `id="email"`, `type="email"`, `required`
    - Nút submit: `<button type="submit" class="btn-submit">Thêm sinh viên</button>`

- [ ] **Step 4: Viết khối chân trang (Footer)**
  - Chú thích: `<!-- ================= CHÂN TRANG ================= -->`
  - Thẻ `<footer class="footer">` chứa đoạn văn `<p class="footer-text">Rikkei Student Admin - Dữ liệu minh họa</p>`.

---

### Task 2: Xây dựng CSS Core, Biến Hệ Thống & Hiệu Ứng Tương Tác (`style.css`)

**Files:**
- Create: `c:/Users/PC/Desktop/AI Driven/Stage-4-BTVN-6/style.css`

**Interfaces:**
- Consumes: `index.html` elements
- Produces: Toàn bộ CSS variables `:root`, reset, layout cơ bản, trạng thái tương tác `:hover` & `:focus-visible`.

- [ ] **Step 1: Định nghĩa CSS Variables (`:root`)**
  - Đầy đủ màu sắc thương hiệu, màu trạng thái (xanh, hổ phách, đỏ, tím), màu nền trang `#f8f9fa`, màu card `#ffffff`, màu viền `#e5e7eb`.
  - Đầy đủ thang khoảng cách (`--space-xs` đến `--space-2xl`) và bo góc (`--radius-sm` đến `--radius-full`).
  - Đổ bóng và vòng sáng tiêu điểm (`--shadow-card`, `--focus-ring`).

- [ ] **Step 2: Thiết lập CSS Reset & Base Styles**
  - `box-sizing: border-box`, `margin: 0`, `padding: 0`.
  - Phông chữ hệ thống hiển thị tiếng Việt sắc nét.
  - Định nghĩa style tương tác không làm dịch chuyển kích thước:
    - Focus visible: `outline: 2px solid var(--color-primary); outline-offset: 2px;` hoặc `box-shadow: var(--focus-ring);`.
    - Tránh thay đổi `padding`, `border-width` khi hover/focus.

---

### Task 3: Định dạng Thanh trên (Topbar) & Menu bên (Sidebar)

**Files:**
- Modify: `c:/Users/PC/Desktop/AI Driven/Stage-4-BTVN-6/style.css`

**Interfaces:**
- Consumes: Task 2 base styles
- Produces: Thanh trên sticky cố định và sidebar navigation đúng màu và font.

- [ ] **Step 1: Khối CSS Thanh trên với chú thích tiếng Việt**
  - Dòng chú thích tiếng Việt: `/* Khối thanh trên: thanh tiêu đề cố định trên cùng, chứa thương hiệu, lời chào và nút menu */`
  - `position: sticky; top: 0; z-index: 100;`
  - Màu nền tím đậm `#22094b`, căn chỉnh flexbox giữa brand và user info/hamburger.
  - Avatar người dùng hình tròn nền tím `#6c2bd9` chữ trắng `L`.
  - Nút hamburger icon màu trắng, căn giữa các vạch kẻ.

- [ ] **Step 2: Khối CSS Menu bên với chú thích tiếng Việt**
  - Dòng chú thích tiếng Việt: `/* Khối menu bên: danh mục điều hướng chính của trang quản trị */`
  - Độ rộng cố định 200px, màu nền tím đậm `#22094b`.
  - Mục active "Tổng quan": nền tím tím sáng `#6c2bd9`, chữ trắng, bo góc 6px.
  - Các mục khác: chữ trắng mờ `rgba(255, 255, 255, 0.7)`, hover nền mờ `rgba(255, 255, 255, 0.08)`.

---

### Task 4: Định dạng Vùng Nội dung Chính (Thống kê, Danh sách, Form)

**Files:**
- Modify: `c:/Users/PC/Desktop/AI Driven/Stage-4-BTVN-6/style.css`

**Interfaces:**
- Consumes: Task 2, 3 styles
- Produces: Thẻ thống kê 4 cột, danh sách sinh viên dạng list căn cột thẳng tắp, form thêm sinh viên 2 cột.

- [ ] **Step 1: Khối CSS Thống kê với chú thích tiếng Việt**
  - Dòng chú thích tiếng Việt: `/* Khối thống kê: 4 thẻ hiển thị các chỉ số tổng quan với dải màu biên trái */`
  - Grid 4 cột (`repeat(4, 1fr)`).
  - Thẻ nền trắng, viền mảnh, bo góc 8px.
  - Dải màu biên trái 4px: tím (Tổng SV), xanh lá (Đang học), vàng hổ phách (Bảo lưu), đỏ (Nghỉ học).
  - Số liệu to nổi bật (~28px-32px), link "Xem danh sách" màu tím với hover đổi màu hoặc gạch chân.

- [ ] **Step 2: Khối CSS Danh sách sinh viên với chú thích tiếng Việt**
  - Dòng chú thích tiếng Việt: `/* Khối danh sách sinh viên: hiển thị các sinh viên mới nhập học dạng danh sách */`
  - Cấu trúc list `<ul>` / `<li>`.
  - Thẻ `<li>` dùng CSS Grid: `display: grid; grid-template-columns: 40px minmax(170px, 1.2fr) minmax(200px, 1.5fr) auto; align-items: center; gap: 16px;`.
  - Đảm bảo khi tên rất dài hoặc thêm dòng mới, toàn bộ 4 cột vẫn thẳng tắp 100%.
  - Avatar ký tự vuông bo góc nhẹ nền `#ede9fe` chữ `#6c2bd9`.
  - Badge trạng thái con nhộng (`border-radius: 9999px`), màu nền và chữ tương ứng từng trạng thái.

- [ ] **Step 3: Khối CSS Form với chú thích tiếng Việt**
  - Dòng chú thích tiếng Việt: `/* Khối form: biểu mẫu thêm nhanh sinh viên mới với các ô nhập liệu */`
  - Thẻ card nền trắng, bo góc 8px.
  - Grid 2 cột cho các trường Họ tên / Mã SV và Lớp / Email.
  - Input, select bo góc 6px, viền xám nhạt, focus viền tím và shadow mờ.
  - Nút "Thêm sinh viên": nền tím `#6c2bd9`, hover `#5b21b6`, padding 10px 24px, bo góc 6px.

---

### Task 5: Định dạng Chân trang & Thiết kế Responsive (< 768px, 360px, 320px)

**Files:**
- Modify: `c:/Users/PC/Desktop/AI Driven/Stage-4-BTVN-6/style.css`

**Interfaces:**
- Consumes: Task 1 - 4
- Produces: Chân trang và responsive hoàn thiện ở mọi kích thước màn hình.

- [ ] **Step 1: Khối CSS Chân trang với chú thích tiếng Việt**
  - Dòng chú thích tiếng Việt: `/* Khối chân trang: dòng thông tin bản quyền và lưu ý dữ liệu minh họa */`
  - Căn giữa, chữ xám nhạt `#6b7280`, padding 24px 0.

- [ ] **Step 2: Responsive cho màn hình Tablet & Mobile (< 768px & 360px)**
  - `@media (max-width: 767px)`:
    - Ẩn sidebar: `display: none;`
    - Ẩn user greeting & avatar: `display: none;`
    - Hiển thị nút hamburger ba gạch: `display: flex;`
    - Thẻ thống kê: chuyển thành 2 cột (`grid-template-columns: repeat(2, 1fr)`).
    - Danh sách sinh viên: ẩn cột Email (`display: none;`), grid chuyển sang `grid-template-columns: 40px 1fr auto;`.
    - Form: chuyển thành 1 cột (`grid-template-columns: 1fr;`), nút submit mở rộng full-width (`width: 100%;`).
  - Kiểm tra đặc biệt ở 320px: Đảm bảo không bị tràn bề ngang (`overflow-x: hidden` hoặc mọi phần tử co giãn vừa vặn).

---

### Task 6: Kiểm tra Xác minh (Verification) theo 9 Tiêu chí

**Files:**
- Review: `index.html`, `style.css`

- [ ] **Step 1: Kiểm tra HTML Semantics và Heading Hierarchy**
  - Xác nhận chỉ có 1 `<h1>` và các `<h2>` đúng ngữ nghĩa.
  - Xác nhận danh sách sinh viên dùng `<ul>/<li>`, không dùng `<table>`.
  - Xác nhận `for` và `id` của form controls khớp nhau 100%.

- [ ] **Step 2: Kiểm tra Form Validation**
  - Thử nghiệm trạng thái rỗng và submit để đảm bảo browser chặn.
  - Thử nghiệm định dạng email.

- [ ] **Step 3: Kiểm tra Responsive ở 1280px, 768px, 360px và 320px**
  - Khớp ảnh mẫu 1280px trong dung sai 2px.
  - Khớp ảnh mẫu 360px trong dung sai 2px.
  - Ở 320px: không có thanh cuộn ngang.

- [ ] **Step 4: Kiểm tra Sinh viên thứ 6 và Tên Dài**
  - Thêm một sinh viên thứ 6 vào markup danh sách.
  - Đổi họ tên thành tên dài: "Nguyễn Hoàng Triệu Nhật Minh Khang".
  - Xác nhận các cột ở 1280px vẫn thẳng hàng đều đặn.

- [ ] **Step 5: Kiểm tra Hiệu ứng tương tác (:hover, :focus-visible)**
  - Tab qua từng link, button, input: focus ring hiển thị, không giật layout.

