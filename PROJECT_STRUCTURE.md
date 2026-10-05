# Cấu trúc dự án nanad017.github.io

Tài liệu này giải thích vai trò của các thư mục và file quan trọng trong blog. Dự án dùng Astro làm bộ máy build và Navfolio làm bộ giao diện, trang mẫu và hệ thống nội dung.

## Luồng hoạt động

```text
src/config/site.toml + src/content/**/*
                 ↓
         Astro + Navfolio
                 ↓
       HTML/CSS/JS trong dist/
                 ↓
            GitHub Pages
```

Phần lớn thời gian chỉ cần chỉnh `src/config/site.toml`, viết nội dung trong `src/content/` và đặt ảnh trong `public/`.

## Cây thư mục chính

```text
nanad017.github.io/
├── .github/workflows/       # Tự động build và deploy GitHub Pages
├── .husky/                  # Git hook chạy kiểm tra trước khi commit
├── public/                  # File tĩnh được chép nguyên trạng ra website
├── scripts/                 # Script tạo nội dung và xử lý font
├── src/
│   ├── assets/              # Ảnh được Astro xử lý khi build
│   ├── components/          # Các khối giao diện Astro
│   ├── config/              # Thông tin và tùy chọn của website
│   ├── content/             # Bài viết và dữ liệu của blog
│   ├── data/                # Đọc và chuẩn hóa cấu hình
│   ├── i18n/                # Chuỗi giao diện theo ngôn ngữ
│   ├── layouts/             # Khung trang dùng chung
│   ├── modules/             # Route do module Navfolio cung cấp
│   ├── pages/               # Các URL của Astro
│   ├── plugins/             # Nối Navfolio với Astro
│   ├── styles/              # CSS và khai báo font toàn cục
│   ├── utils/               # Hàm hỗ trợ tìm kiếm, ngày tháng, TOC...
│   └── content.config.ts    # Schema kiểm tra frontmatter
├── astro.config.mjs         # Cấu hình Astro, MDX, sitemap và Tailwind
├── navfolio.config.ts       # Bật/tắt module và plugin Navfolio
├── package.json             # Danh sách dependency và lệnh Bun
├── bun.lock                 # Khóa chính xác phiên bản dependency
├── ec.config.mjs            # Cấu hình khối code Expressive Code
├── eslint.config.js         # Quy tắc kiểm tra TypeScript/Astro
├── pagefind.yml             # Cấu hình tìm kiếm tĩnh Pagefind
├── tsconfig.json            # Cấu hình TypeScript
└── vercel.json              # Cấu hình nếu triển khai bằng Vercel
```

## Những nơi thường chỉnh sửa

### `src/config/site.toml`

Đây là file cấu hình cá nhân chính:

- Tên, mô tả và URL website.
- Tên, avatar, email và liên kết GitHub.
- Menu trên cùng.
- Nội dung các thẻ ở trang chủ.
- Màu giao diện, ngôn ngữ, font và tìm kiếm.
- Tiêu đề của Blog, Projects, Vibe và Media.
- Cấu hình bình luận.

### `src/content/`

Nội dung được chia thành các collection:

```text
src/content/
├── about.mdx                # Nội dung trang /about, bắt buộc
├── blog/                    # Bài blog, mỗi file tạo một URL
├── projects/
│   ├── index.mdx            # Lời giới thiệu trang /projects, bắt buộc
│   └── *.mdx                # Từng dự án
├── vibe/                    # Ghi chú ngắn, trạng thái, ảnh
└── media/                   # Sách, phim, nhạc, podcast
```

Mỗi file có hai phần:

1. Frontmatter nằm giữa hai dấu `---`, chứa tiêu đề, ngày, tag và tùy chọn hiển thị.
2. Nội dung Markdown hoặc MDX nằm phía dưới.

`src/content.config.ts` định nghĩa trường nào hợp lệ và sẽ báo lỗi build nếu frontmatter sai.

### `src/pages/`

Astro dùng cấu trúc file để tạo URL:

- `src/pages/index.astro` → `/`
- `src/pages/about.astro` → `/about`
- `src/pages/blog/index.astro` → `/blog`
- `src/pages/blog/[...slug].astro` → từng bài blog
- `src/pages/rss.xml.js` → `/rss.xml`

Projects, Vibe và Media được đăng ký qua `@navfolio/pages` nên một phần route nằm trong `src/modules/` và package Navfolio.

### `src/components/` và `src/layouts/`

- `components/` chứa các phần nhỏ như header, footer, card, tìm kiếm và mục lục.
- `layouts/` ghép các component thành khung trang hoàn chỉnh.
- File `.astro` có thể chứa frontmatter TypeScript, HTML template, script và CSS scoped trong cùng một file.

Chỉ cần sửa hai thư mục này khi muốn thay đổi giao diện hoặc hành vi của template.

### `public/` và `src/assets/`

- `public/`: dùng cho favicon, avatar, font và ảnh muốn truy cập bằng URL cố định, ví dụ `public/images/avatar.webp` sẽ có URL `/images/avatar.webp`.
- `src/assets/`: dùng khi import ảnh vào component hoặc MDX để Astro tối ưu ảnh trong lúc build.

Không đặt thông tin bí mật trong hai thư mục này vì mọi file đều có thể xuất hiện trên website.

### `navfolio.config.ts`

File này đang bật:

- Projects.
- Vibe.
- Media.
- Markdown nâng cao.
- Expressive Code.
- Công thức toán.
- Mermaid.
- Bảng responsive.

Đây là nơi thay đổi module cấp cao. Nội dung cá nhân thông thường không cần sửa file này.

### `.github/workflows/deploy-pages.yml`

Workflow chạy khi có commit được push lên nhánh `main`:

1. Cài Node và Bun.
2. Cài dependency theo `bun.lock`.
3. Build website.
4. Upload thư mục `dist/`.
5. Deploy lên GitHub Pages.

## Thư mục được sinh tự động

Các thư mục sau không phải mã nguồn và đã nằm trong `.gitignore`:

- `node_modules/`: toàn bộ thư viện được cài bởi Bun.
- `.astro/`: cache và type do Astro sinh ra.
- `dist/`: website đã build.

Có thể tạo lại chúng bằng `bun install` và `bun run build`; không cần commit lên Git.

## Những file nên giữ

- Giữ `bun.lock` để máy local và GitHub Actions dùng cùng phiên bản thư viện.
- Giữ `package.json` vì nó định nghĩa dependency và các lệnh của dự án.
- Giữ `about.mdx` và `projects/index.mdx`; template hiện yêu cầu hai file này khi build.
- Không sửa trực tiếp file bên trong `node_modules/`; thay đổi sẽ mất sau lần cài dependency tiếp theo.

Xem [USAGE_GUIDE.md](./USAGE_GUIDE.md) để chạy dự án, viết bài và deploy.
