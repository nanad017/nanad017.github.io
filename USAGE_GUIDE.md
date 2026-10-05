# Hướng dẫn sử dụng blog Navfolio

Đây là hướng dẫn thực hành cho dự án `nanad017.github.io`, từ chạy local đến đăng website lên GitHub Pages.

## 1. Chạy dự án trên máy

Mở terminal và đi vào thư mục dự án:

```bash
cd /home/nad/Myself/blog/nanad017.github.io
```

Cài dependency lần đầu hoặc sau khi `package.json`/`bun.lock` thay đổi:

```bash
bun install
```

Chạy chế độ phát triển:

```bash
bun run dev
```

Mở địa chỉ Astro in ra terminal, thông thường là:

```text
http://localhost:4321/
```

Khi sửa file, trình duyệt sẽ tự cập nhật. Nhấn `Ctrl+C` trong terminal để dừng server.

Nếu server được chạy nền bằng Astro:

```bash
bunx astro dev status
bunx astro dev logs
bunx astro dev stop
```

## 2. Chỉnh thông tin cá nhân

Mở `src/config/site.toml` và chỉnh các nhóm sau:

- `[config.site]`: tên, mô tả, URL và repository.
- `[config.profile]`: tên, username, nghề nghiệp, email, website, GitHub và avatar.
- `[[config.topNav.links]]`: menu trên đầu trang.
- `[config.home.quote]`: câu giới thiệu lớn.
- `[config.home.intro]`: phần giới thiệu cá nhân.
- `[[config.home.navigation]]`: các thẻ điều hướng.
- `[[config.home.links]]`: mạng xã hội; để `tooltip = ""` nếu muốn ẩn.
- `[[config.home.doing]]`: danh sách việc đang làm.

Đổi màu giao diện bằng:

```toml
[config.theme]
palette = "green-soft"
lang = "en"
```

Các palette có sẵn gồm `green-soft`, `green-vivid`, `rose-soft`, `pink-soft`, `purple-soft`, `blue-soft`, `orange-soft` và `brown-soft`.

## 3. Viết bài blog

Tạo một bài mới bằng script có sẵn:

```bash
bun run post:new ten-bai-viet
```

File được tạo tại `src/content/blog/ten-bai-viet.md`. URL sau khi đăng là `/blog/ten-bai-viet/`.

Muốn dùng MDX:

```bash
bun run post:new ten-bai-viet --mdx
```

Mẫu bài:

```md
---
title: 'Tiêu đề bài viết'
description: 'Mô tả ngắn cho danh sách bài và SEO.'
date: '2026-10-05'
draft: false
sticky: false
showHeroImage: false
tags: ['astro', 'blog']
categories: ['web']
series: []
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# Tiêu đề bài viết

Nội dung viết ở đây.
```

Các trường thường dùng:

- `draft: true`: giữ bài ở trạng thái nháp, không đăng công khai.
- `sticky: true`: ghim bài trong danh sách.
- `tags`: nhãn chi tiết của bài.
- `categories`: nhóm chủ đề lớn.
- `series`: chuỗi bài liên quan.
- `comments`: bật khu vực bình luận nếu website đã cấu hình nhà cung cấp bình luận.
- `sidebar.toc`: bật mục lục theo heading.

## 4. Viết MDX

MDX là Markdown có thể dùng component. File dùng đuôi `.mdx`.

Ví dụ đơn giản:

```mdx
---
title: 'Bài MDX đầu tiên'
description: 'Thử Markdown kết hợp component.'
date: '2026-10-05'
draft: false
showHeroImage: false
---

# Markdown bình thường

Bạn vẫn viết **in đậm**, danh sách và khối code như Markdown.

<div class="note">Đây là HTML nằm trong MDX.</div>
```

Chỉ dùng MDX khi cần component hoặc HTML động; bài viết thông thường có thể dùng `.md`.

## 5. Thêm dự án, ghi chú và media

Tạo nội dung bằng các lệnh:

```bash
bun run project:new ten-du-an
bun run vibe:new ghi-chu-hom-nay
bun run media:new ten-sach-hoac-phim
```

Vị trí file và URL:

| Loại    | Thư mục                 | URL                     |
| ------- | ----------------------- | ----------------------- |
| Blog    | `src/content/blog/`     | `/blog/<ten-file>/`     |
| Project | `src/content/projects/` | `/projects/<ten-file>/` |
| Vibe    | `src/content/vibe/`     | Hiển thị trong `/vibe`  |
| Media   | `src/content/media/`    | Hiển thị trong `/media` |

Lưu ý:

- `src/content/projects/index.mdx` là phần giới thiệu của trang Projects, không phải một project riêng.
- `src/content/about.mdx` là nội dung trang About.
- Có thể xem các file mẫu hiện có trong từng thư mục trước khi tạo nội dung mới.

## 6. Thêm ảnh

Cách đơn giản nhất là đặt ảnh vào `public/images/`:

```text
public/images/anh-bai-viet.webp
```

Sau đó dùng trong Markdown/MDX:

```md
![Mô tả ảnh](/images/anh-bai-viet.webp)
```

Tên file nên viết thường, không dấu và dùng dấu gạch ngang. Nên dùng WebP hoặc AVIF để giảm dung lượng.

## 7. Chỉnh giao diện

Các vị trí chính:

- `src/styles/global.css`: màu, khoảng cách và CSS toàn cục.
- `src/styles/fonts.css`: font chữ.
- `src/components/`: từng khối giao diện.
- `src/layouts/`: khung trang bài viết và trang chung.
- `src/config/site.toml`: lựa chọn màu/theme mà không cần sửa component.

Template có Tailwind nhưng phần lớn giao diện hiện tại nằm trong component Astro và CSS. Không cần React hoặc Next.js để chỉnh site này.

## 8. Kiểm tra trước khi đăng

Kiểm tra lint và format:

```bash
bun run format:check
```

Build bản production:

```bash
bun run build
```

Nếu build thành công, kết quả nằm trong `dist/`. Xem thử bản production:

```bash
bun run preview
```

Không cần commit thư mục `dist/`.

## 9. Đăng lên GitHub Pages

Workflow đã được cấu hình tại `.github/workflows/deploy-pages.yml` và chạy khi push nhánh `main`.

Kiểm tra thay đổi:

```bash
git status
git diff
```

Commit và push:

```bash
git add .
git commit -m "Set up Navfolio personal blog"
git push origin main
```

Trong repository GitHub, vào **Settings → Pages** và chọn **GitHub Actions** làm nguồn deploy. Sau đó theo dõi workflow trong tab **Actions**.

Website sẽ được xuất bản tại:

```text
https://nanad017.github.io
```

Không đặt token, password hoặc secret trong source code. Secret dùng cho workflow phải lưu trong phần Settings của GitHub repository.

## 10. Quản lý dependency

Cài lại dependency theo lockfile:

```bash
bun install --frozen-lockfile
```

Thêm một thư viện:

```bash
bun add ten-thu-vien
```

Thêm thư viện chỉ dùng khi phát triển:

```bash
bun add --dev ten-thu-vien
```

Gỡ thư viện:

```bash
bun remove ten-thu-vien
```

Dependency chỉ nằm trong `node_modules/`. Không có môi trường `venv`; xóa `node_modules/` rồi chạy `bun install` sẽ tạo lại toàn bộ.

## 11. Lỗi thường gặp

### Cổng 4321 đang được dùng

Astro sẽ tự chọn cổng khác hoặc có thể dừng server nền:

```bash
bunx astro dev stop
```

### Build báo thiếu `about.mdx`

Kiểm tra file sau còn tồn tại:

```text
src/content/about.mdx
```

### Build báo thiếu `projects/index.mdx`

Kiểm tra file:

```text
src/content/projects/index.mdx
```

### Frontmatter không hợp lệ

Đối chiếu với bài mẫu và schema trong `src/content.config.ts`. Các lỗi thường gặp là ngày sai định dạng, URL không hợp lệ hoặc viết sai tên trường.

### Tìm kiếm không hoạt động khi chạy dev

Pagefind được tạo trong quá trình production build. Chạy:

```bash
bun run build
bun run preview
```

## Tài liệu liên quan

- [PROJECT_STRUCTURE.md](./PROJECT_STRUCTURE.md): giải thích cấu trúc và vai trò của từng phần.
- `README.md`: tài liệu gốc của Navfolio.
- `CHANGELOG.md`: lịch sử thay đổi của template.
