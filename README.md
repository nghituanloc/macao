# macao

Tài liệu này hướng dẫn triển khai giao diện macOS để code qua GitHub Actions và Ngrok, từ khâu chuẩn bị đến kết nối VNC.

## Yêu cầu

- Tài khoản GitHub
- Tài khoản ngrok và authtoken
- Git cài trên máy
- VNC Viewer trên Windows hoặc Screen Sharing trên macOS

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

## Bước 2: Tạo secret NGROK_AUTH_TOKEN

Trên GitHub:
- Vào repo -> Settings -> Secrets and variables -> Actions -> New repository secret
- Name: `NGROK_AUTH_TOKEN`
- Value: dán authtoken lấy từ trang ngrok

## Bước 3: Chạy workflow

Trên GitHub:
- Vào tab Actions
- Chọn workflow `macOS GUI via Ngrok`
- Bấm Run workflow

Không cần cấu hình self-hosted runner. Workflow dùng runner do GitHub cung cấp (`macos-latest`).

## Bước 4: Lấy địa chỉ VNC

Mở log của bước `Start Ngrok Tunnel`. Ở cuối log sẽ in ra địa chỉ dạng:

```
tcp://0.tcp.ngrok.io:12345
```

## Bước 5: Kết nối VNC

Trên Windows:
- Mở VNC Viewer
- Nhập địa chỉ ở log, bỏ phần `tcp://`

Trên macOS:
- Mở ứng dụng Screen Sharing
- Nhập địa chỉ ở log, bỏ phần `tcp://`

Nếu không kết nối được, chạy lại workflow và lấy địa chỉ mới.

## Thư mục mã nguồn trên macOS runner

Sau khi kết nối, mã nguồn nằm tại:

```
/Users/runner/work/<repo>/<repo>
```

## Kết thúc phiên làm việc

- Hủy workflow trong tab Actions khi xong việc để giải phóng tài nguyên.
- Phiên sẽ tự dừng sau 6 giờ do lệnh `sleep 21600`.

## Lưu ý bảo mật

- Nếu authtoken bị lộ, hãy thu hồi token cũ và tạo token mới.
- Không lưu token vào file trong repo.

## Xử lý sự cố thường gặp

- Không có địa chỉ ngrok: kiểm tra secret `NGROK_AUTH_TOKEN` và chạy lại workflow.
- Không kết nối VNC: thử lại sau 1 đến 2 phút, lấy địa chỉ ngrok mới.
- Workflow bị tắt sớm: GitHub giới hạn thời gian chạy tối đa.
