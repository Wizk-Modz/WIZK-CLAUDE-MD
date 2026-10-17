# Quy Tắc Phát Triển

Task chỉ hoàn thành khi đã self-review kỹ và khắc phục toàn bộ lỗi.

## 1. Quy trình
**PLAN → BRANCH → IMPLEMENT → SELF-REVIEW → FIX → VERIFY → REPORT**
- Task nhỏ (typo, sửa 1 dòng, docs): bỏ PLAN/BRANCH, vẫn bắt buộc SELF-REVIEW → VERIFY.
- Chỉ tạo branch khi thư mục là git repo; chưa init → hỏi trước khi `git init`.
- Sửa bug: tái hiện lỗi trước, sửa đúng root cause (không vá triệu chứng), kiểm tra lại các flow liên quan.
- Lặp *Find & Fix* đến khi sạch lỗi.
- Không chỉ báo cáo lỗi mà không sửa. Không thay đổi ngoài scope task.
- Lỗi không sửa được → rollback, thông báo lý do. Không để code nửa chừng.
- Task quá lớn → đề xuất tách nhỏ. Dependency xung đột → báo cụ thể version.

## 2. Dự án mới → MVP (Minimum Viable Product)
- Khi bắt đầu dự án mới: xác định phạm vi **MVP** trước — phiên bản nhỏ nhất chạy được, chỉ gồm các chức năng cốt lõi giải quyết đúng bài toán chính.
- Làm MVP chạy end-to-end thật trước; không thêm tính năng phụ, thư viện hay tối ưu sớm khi MVP chưa hoàn chỉnh (tránh over-engineering).
- MVP quá lớn → đề xuất tách thành nhiều MVP nhỏ hơn, bàn giao theo từng iteration.

## 3. Comment & Đặt tên
- Mỗi hàm bắt buộc **1 dòng comment** ngay trên định nghĩa, mô tả ngắn gọn chức năng.
- Comment phải ngắn gọn và chỉ mô tả trực tiếp nội dung cần thiết. Không thêm phần tham chiếu như `(khớp app.py tham chiếu)`, `(theo app.py)`, `(xem file X)`, v.v. nếu không được yêu cầu.
- Dùng ký hiệu comment đúng ngôn ngữ (`#` Python/Ruby, `//` JS/TS/Java/Go/Rust/C/PHP, `--` SQL/Lua).
- **Comment tiếng Việt (hoặc ngôn ngữ có dấu) phải viết đầy đủ dấu** — tuyệt đối không viết không dấu.
  - ✅ Đúng: `# Thư mục gốc của repo (tools/ nằm trong đó)`
  - ❌ Sai: `# Thu muc goc cua repo (tools/ nam trong do)`
- Đặt tên theo convention chuẩn của ngôn ngữ đang dùng (snake_case, camelCase, PascalCase tương ứng).
- Tên rõ nghĩa, tránh viết tắt mơ hồ (trừ `i`, `ctx`, `err`, `req`, `res`, `db`, `tx`).
- **Ưu tiên convention dự án hiện có** hơn convention mặc định.

## 4. Cấu trúc & Dependency
- Tạo file/thư mục theo cấu trúc chuẩn của ngôn ngữ. Dự án có sẵn cấu trúc → tuân theo, không tự ý tổ chức lại.
- Không tự ý thêm/nâng cấp dependency. Ghi rõ version, tránh `latest`.
- Lock file phải đồng bộ. Kiểm tra tương thích trước khi thêm dependency mới.
- File > 300 dòng → cân nhắc tách module. Sửa một phần → dùng chỉnh sửa cục bộ.

## 5. Review & Checklist
- **Chức năng**: Logic, edge cases, null/exception, nhất quán trạng thái.
- **Chất lượng**: Không dead code / unused import, đặt tên chuẩn.
- **Bảo mật**: Validate input, không hardcode secrets, tránh leak tài nguyên.
- **Build**: Syntax, import, typecheck/lint, tests pass.
- **Tương thích**: Không phá vỡ API/interface hiện có.

Checklist:
- [ ] Build/lint/typecheck OK
- [ ] Không broken import, không sót implementation
- [ ] Không TODO/FIXME còn sót (trừ khi yêu cầu)
- [ ] Không còn debug log / print tạm
- [ ] Tests pass, config hợp lệ
- [ ] Không xoá/ghi đè file ngoài scope
- [ ] Dependency không xung đột, lock file đồng bộ

## 6. Build & Test
- Chạy build/lint/test bằng tool đã cấu hình trong dự án — không tự thêm tool mới.
- Dự án không có test/lint → ghi nhận trong báo cáo, không tự tạo trừ khi được yêu cầu.

## 7. Báo cáo cuối (bắt buộc)
```
Review Summary
Changes Made:
- ...
Files Reviewed:
- ...
Issues Found: N
Issues Fixed:
- ...
Validation Performed:
- ...
Remaining Risks: None
```

## 8. Commit Message
Tiền tố: `feat:` | `fix:` | `style:` | `refactor:` | `perf:` | `test:` | `docs:` | `ci:` | `chore:`
- Định dạng: `<tiền tố>(<scope>): <mô tả ngắn ở thể mệnh lệnh>`.
- Ví dụ: `feat(ocr): thêm nhận dạng PDF nhiều trang`.

## 9. Quy tắc cốt lõi
- **Không tự ý commit** khi chưa được yêu cầu.
- **Không xoá/ghi đè file** ngoài scope task.
- **Tuyệt đối không commit secrets/credentials.**
- **Theo style & tư duy dự án**: code theo đúng style, pattern, tư duy hiện có của dự án — chỉ thay đổi khi user yêu cầu rõ ràng.
- **Ngôn ngữ**: Trả lời bằng tiếng Việt trừ khi yêu cầu khác.
- **Không chắc chắn** → hỏi lại, không tự đoán.
- Luôn self-review kỹ; có lỗi là sửa triệt để ngay.
- **Code phải hoạt động đúng chức năng, không chỉ compile được.** Luôn kiểm chứng logic, chạy thử hoặc trace qua flow để đảm bảo kết quả đúng — không chỉ dừng ở build/lint pass.
