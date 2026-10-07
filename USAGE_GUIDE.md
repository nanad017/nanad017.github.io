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

| Tệp                    | Chức năng                                                                                                                                                                             |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `.editorconfig`        | Chuẩn hoá cách trình soạn thảo lưu mã: UTF-8, xuống dòng LF, thụt lề 2 dấu cách, xoá khoảng trắng cuối dòng.                                                                          |
| `.gitignore`           | Bảo Git không theo dõi các tệp/thư mục sinh tự động hoặc riêng tư như `node_modules/`, `dist/`, `.astro/`, `.venv/`, `.env`, cache font và dữ liệu Friend Circle.                     |
| `.gitmodules`          | Khai báo `src/docs` là Git submodule, lấy nội dung từ kho `astro-navfolio-docs`.                                                                                                      |
| `.npmrc`               | Đặt npm registry thành mirror `https://registry.npmmirror.com` khi cài package.                                                                                                       |
| `.nvmrc`               | Chỉ định phiên bản Node.js nên dùng: `22.12.0`.                                                                                                                                       |
| `.prettierignore`      | Loại trừ các tệp build, dependency, lockfile, favicon và nội dung Markdown/MDX khỏi Prettier. Markdown/MDX bị loại trừ để giữ nguyên kiểu dấu nháy trong frontmatter do tác giả chọn. |
| `.prettierrc`          | Quy tắc định dạng bằng Prettier: 2 spaces, dấu chấm phẩy, nháy đơn, tối đa 100 ký tự/dòng và hỗ trợ định dạng `.astro`.                                                               |
| `AGENT.md`             | Hướng dẫn cho AI agent hoặc người bảo trì: kiến trúc Navfolio, phạm vi các package, nguyên tắc sửa đổi và lệnh kiểm tra. Không ảnh hưởng trực tiếp tới website khi build.             |
| `astro.config.mjs`     | Cấu hình trung tâm của Astro: đọc URL từ `site.toml`/biến môi trường, xử lý base path cho GitHub Pages, bật MDX, sitemap, Tailwind, Markdown plugin và ánh xạ runtime của Navfolio.   |
| `bun.lock`             | Lockfile của Bun: khoá chính xác phiên bản và nguồn dependency để máy local/CI cài cùng một bộ thư viện. Không nên sửa tay.                                                           |
| `CHANGELOG.md`         | Nhật ký thay đổi theo phiên bản; hiện ghi các cập nhật của v0.1.0 và v0.2.0.                                                                                                          |
| `CONTRIBUTING.md`      | Hướng dẫn đóng góp cho Navfolio: quy tắc tạo PR, kiểm tra local, nơi nên đặt thay đổi và quy trình với docs submodule.                                                                |
| `ec.config.mjs`        | Cấu hình Expressive Code cho khối mã trong Markdown: theme sáng/tối, số dòng, xuống dòng, vùng code thu gọn, màu sắc và kiểu khung.                                                   |
| `eslint.config.js`     | Cấu hình ESLint, hiện tập trung lint TypeScript/TSX, dùng `tsconfig.json` và báo lỗi khi dùng API TypeScript đã deprecated.                                                           |
| `LICENSE`              | Giấy phép MIT: cho phép sử dụng, sửa, phân phối và bán lại phần mềm, với điều kiện giữ thông báo bản quyền và miễn trừ bảo hành.                                                      |
| `navfolio.config.ts`   | Bật và cấu hình các module Navfolio: Projects, Vibe, Media, Pages và Markdown nâng cao (code block, toán học, Mermaid, bảng responsive…).                                             |
| `package.json`         | Manifest dự án: tên, phiên bản, yêu cầu Node, scripts (`dev`, `build`, `lint`, tạo bài viết…), dependencies và devDependencies.                                                       |
| `pagefind.yml`         | Cấu hình Pagefind tạo chỉ mục tìm kiếm tĩnh từ thư mục `dist`, chỉ tìm nội dung trong thẻ `main`, dùng tiếng Trung làm ngôn ngữ mặc định và bỏ qua vùng có `data-pagefind-ignore`.    |
| `PROJECT_STRUCTURE.md` | Tài liệu tiếng Việt giải thích cấu trúc thư mục, luồng build/deploy và các vị trí thường chỉnh như `src/config/site.toml`, `src/content/`, `public/`.                                 |
| `README.en.md`         | Tài liệu giới thiệu và hướng dẫn sử dụng Navfolio bằng tiếng Anh.                                                                                                                     |
| `README.md`            | Tài liệu giới thiệu và hướng dẫn sử dụng Navfolio bằng tiếng Trung, gồm cài đặt, cấu hình, nội dung, routes, search và comments.                                                      |
| `tsconfig.json`        | Cấu hình TypeScript: kế thừa chế độ strict của Astro, kiểm tra toàn bộ mã nguồn và loại trừ `dist/`.                                                                                  |
| `USAGE_GUIDE.md`       | Hướng dẫn tiếng Việt thực hành cho blog này: chạy local, tạo bài viết/dự án/vibe/media, thêm ảnh, đổi giao diện, build và deploy GitHub Pages.                                        |
| `vercel.json`          | Cấu hình deploy Vercel: tạo Python virtual environment, cài FontTools/Brotli và Bun dependencies, cập nhật submodule, build docs rồi xuất website vào `dist/`.                        |

| Thư mục         | Chức năng                                                                                                                                                                                 |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `.agents/`      | Lưu ngữ cảnh, quy ước thiết kế và workflow dành cho AI agent/bảo trì dự án. Không được đưa vào website khi build.                                                                         |
| `.astro/`       | Cache và các TypeScript type do Astro tự sinh trong lúc chạy/build. Có thể xoá rồi tạo lại; không cần commit.                                                                             |
| `.github/`      | Cấu hình dành cho GitHub, thường gồm GitHub Actions workflow để build/deploy lên GitHub Pages, issue/PR templates nếu có.                                                                 |
| `.husky/`       | Git hooks. Trong dự án này hỗ trợ chạy kiểm tra tự động trước những thao tác Git như commit.                                                                                              |
| `.venv/`        | Python virtual environment cục bộ. Dùng để cài `fonttools` và `brotli`, cần cho script tạo font subset khi production build. Không commit.                                                |
| `.vscode/`      | Thiết lập workspace của Visual Studio Code, ví dụ extension gợi ý, cấu hình editor hoặc task. Chỉ ảnh hưởng người dùng VS Code.                                                           |
| `dist/`         | Website đã build sẵn: HTML/CSS/JS, ảnh tối ưu và chỉ mục tìm kiếm Pagefind. Đây là thư mục được deploy, nhưng có thể tạo lại bằng `bun run build`, nên không commit.                      |
| `node_modules/` | Toàn bộ dependency JavaScript/TypeScript đã được Bun cài từ `package.json` và `bun.lock`. Không sửa trực tiếp, không commit; tạo lại bằng `bun install`.                                  |
| `public/`       | Tài nguyên tĩnh công khai. Các file bên trong được chép nguyên trạng sang `dist/`; ví dụ `public/images/avatar.webp` sẽ có URL `/images/avatar.webp`. Không đặt thông tin nhạy cảm ở đây. |
| `scripts/`      | Các script hỗ trợ phát triển, chẳng hạn tạo file nội dung mới và tạo subset font cho site. Được gọi qua các lệnh `bun run ...` trong `package.json`.                                      |
| `src/`          | Mã nguồn chính của blog: component, layout, CSS, route, dữ liệu cấu hình, nội dung Markdown/MDX, schema và các tích hợp Navfolio. Astro xử lý thư mục này để tạo website.                 |

Đổi chữ/link/màu có sẵn → site.toml
Đổi một khối trên giao diện → components/
Đổi vị trí các khối → pages/index.astro hoặc layout/
Đổi CSS chi tiết → styles/
Viết bài → content/
