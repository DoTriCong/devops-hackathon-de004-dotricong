# DevOps Hackathon - Đề 004: Quản lý kho hàng (inventory)

## 1. Thông tin sinh viên
- **Họ và tên**: Đỗ Trí Công
- **Mã sinh viên**: B24DTCN194
- **Lớp**: CNTT2
- **Tài khoản Linux**: tricong
- **GitHub**: https://github.com/DoTriCong
- **Cổng Nginx**: 8080

## 2. Môi trường triển khai
- **Hệ điều hành**: Ubuntu
- **Phiên bản Nginx**: 1.28.3
- **Git**: 2.53.0
- **Nơi chạy**: Máy ảo VPS
- **Địa chỉ IP máy chủ**: 221.121.4.84

## 3. Cấu trúc dự án
```
untitled1/
├── nginx/
│   └── dotricong-k24cntt2.conf    # File cấu hình Nginx
├── src/
│   └── index.html                 # File trang web chính
├── screenshots/
│   ├── 01-user.png                # Screenshot thông tin user
│   ├── 02-nginx.png               # Screenshot cấu hình Nginx
│   └── 03-ufw.png                 # Screenshot cấu hình UFW
├── .gitignore                     # File gitignore
└── README.md                      # File tài liệu dự án
```

## 4. Cấu hình Nginx
- **Port**: 8080 (cổng công khai)
- **server_name**: cong55
- **root**: /var/www/devops-hackathon-de004-dotricong/src
- **index**: index.html
- **Access log**: /var/log/nginx/tricong.access.log
- **Error log**: /var/log/nginx/tricong.error.log

File cấu hình: `nginx/dotricong-k24cntt2.conf`

## 5. Quy tắc UFW (Firewall)
- Cho phép kết nối SSH (port 22)
- Cho phép kết nối HTTP (port 80)
- Cho phép kết nối Nginx (port 8080)
- Chặn các kết nối không cần thiết khác

## 6. Các bước triển khai
1. **Cập nhật hệ thống**:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

2. **Cài đặt Nginx**:
   ```bash
   sudo apt install nginx -y
   ```

3. **Cài đặt Git**:
   ```bash
   sudo apt install git -y
   ```

4. **Clone repository từ GitHub**:
   ```bash
   git clone https://github.com/DoTriCong/devops-hackathon-de004-dotricong.git
   cd devops-hackathon-de004-dotricong
   ```

5. **Sao chép file cấu hình Nginx**:
   ```bash
   sudo cp nginx/dotricong-k24cntt2.conf /etc/nginx/sites-available/
   sudo ln -s /etc/nginx/sites-available/dotricong-k24cntt2.conf /etc/nginx/sites-enabled/
   sudo rm /etc/nginx/sites-enabled/default
   ```

6. **Tạo thư mục web root**:
   ```bash
   sudo mkdir -p /var/www/devops-hackathon-de004-dotricong/src
   sudo cp -r src/* /var/www/devops-hackathon-de004-dotricong/src/
   sudo chown -R www-data:www-data /var/www/devops-hackathon-de004-dotricong/
   sudo chmod -R 755 /var/www/devops-hackathon-de004-dotricong/
   ```

7. **Cấu hình UFW Firewall**:
   ```bash
   sudo ufw allow 22/tcp
   sudo ufw allow 80/tcp
   sudo ufw allow 8080/tcp
   sudo ufw enable
   ```

8. **Khởi động lại Nginx**:
   ```bash
   sudo nginx -t
   sudo systemctl restart nginx
   sudo systemctl enable nginx
   ```

## 7. Kiểm tra và xác thực
1. **Kiểm tra trạng thái Nginx**:
   ```bash
   sudo systemctl status nginx
   ```

2. **Kiểm tra cấu hình Nginx**:
   ```bash
   sudo nginx -t
   ```

3. **Kiểm tra UFW**:
   ```bash
   sudo ufw status
   ```

4. **Truy cập website**:
   - Mở trình duyệt và truy cập: `http://221.121.4.84:8080`
   - Hoặc: `http://cong55:8080`

## 8. Quy trình cập nhật website
1. **Pull code mới từ GitHub**:
   ```bash
   cd /path/to/devops-hackathon-de004-dotricong
   git pull origin main
   ```

2. **Copy file mới vào web root**:
   ```bash
   sudo cp -r src/* /var/www/devops-hackathon-de004-dotricong/src/
   sudo chown -R www-data:www-data /var/www/devops-hackathon-de004-dotricong/
   ```

3. **Reload Nginx**:
   ```bash
   sudo systemctl reload nginx
   ```

## 9. Xử lý sự cố
- **Website không truy cập được**:
  - Kiểm tra trạng thái Nginx: `sudo systemctl status nginx`
  - Kiểm tra cấu hình Nginx: `sudo nginx -t`
  - Kiểm tra firewall: `sudo ufw status`
  - Kiểm tra log: `sudo tail -f /var/log/nginx/tricong.error.log`

- **Lỗi 404**:
  - Kiểm tra đường dẫn file trong cấu hình Nginx
  - Đảm bảo file index.html tồn tại trong thư mục `/var/www/devops-hackathon-de004-dotricong/src/`

- **Lỗi 403 Forbidden**:
  - Kiểm tra quyền truy cập file: `ls -la /var/www/devops-hackathon-de004-dotricong/src/`
  - Đảm bảo quyền là 755 cho thư mục và 644 cho file

- **Nginx không start được**:
  - Kiểm tra port 8080 có đang bị chiếm dụng: `sudo netstat -tulpn | grep 8080`
  - Kiểm tra file cấu hình có lỗi cú pháp: `sudo nginx -t`
