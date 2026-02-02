# macao

Tài liệu này hướng dẫn truy cập macOS runner qua SSH (tmate) bằng GitHub Actions.

## Yêu cầu

- Tài khoản GitHub
- SSH key đã thêm vào GitHub account
- Git cài trên máy
- Terminal có lệnh ssh

## Bước 1: Chuẩn bị repository

Nếu bạn đã có repo chứa file `.github/workflows/macos-gui.yml` thì bỏ qua phần này.

Tạo repo mới trên GitHub, sau đó chạy các lệnh sau ở thư mục dự án:

```
git init
git add .
git commit -m "Add macOS GUI workflow"
git branch -M main
git remote add origin https://github.com/<github-username>/<repo>.git
git push -u origin main
```

Lưu ý:
- Không commit authtoken ngrok vào repo.
- File `.gitignore` đã có dòng `may ao mac.txt` để tránh lộ dữ liệu nhạy cảm nếu bạn có ghi chú cục bộ.

## Bước 2: Thêm SSH key vào GitHub

Trên GitHub:
- Vào Settings -> SSH and GPG keys -> New SSH key
- Dán public key của bạn

Lưu ý:
- Workflow dùng `limit-access-to-actor`, chỉ tài khoản có SSH key mới kết nối được.

## Bước 3: Chạy workflow

Trên GitHub:
- Vào tab Actions
- Chọn workflow `macOS SSH via tmate`
- Bấm Run workflow

Không cần cấu hình self-hosted runner. Workflow dùng runner do GitHub cung cấp (`macos-latest`).

## Bước 4: Lấy lệnh SSH

Mở log của bước `Setup SSH Session`. Ở cuối log sẽ in ra lệnh dạng:

```
ssh <user>@ssh.tmate.io
```

## Bước 5: Kết nối SSH

Trên máy bạn:
- Chạy lệnh SSH ở log
- Nếu SSH key không phải mặc định, dùng `ssh -i path <user>@ssh.tmate.io`

## Thư mục mã nguồn trên macOS runner

Sau khi kết nối, mã nguồn nằm tại:

```
/Users/runner/work/<repo>/<repo>
```

## Kết thúc phiên làm việc

- Hủy workflow trong tab Actions khi xong việc để giải phóng tài nguyên.
- Phiên sẽ tự dừng sau 6 giờ do lệnh `sleep 21600`.
- Thoát SSH để kết thúc sớm.

## Lưu ý bảo mật

- Không chia sẻ lệnh SSH từ log cho người khác.
- Không lưu thông tin nhạy cảm vào repo.

## Xử lý sự cố thường gặp

- Không thấy lệnh SSH: đợi log cập nhật, kiểm tra đã thêm SSH key vào GitHub.
- Không kết nối SSH: kiểm tra mạng, thử chạy lại workflow để tạo phiên mới.
- Workflow bị tắt sớm: GitHub giới hạn thời gian chạy tối đa.
