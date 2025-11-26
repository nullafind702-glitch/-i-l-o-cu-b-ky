# 1. Tạo folder chứa runner
sudo mkdir -p /opt/actions-runner
cd /opt/actions-runner

# 2. Tải runner phiên bản 2.329.0
sudo curl -O -L https://github.com/actions/runner/releases/download/v2.329.0/actions-runner-linux-x64-2.329.0.tar.gz

# 3. Giải nén
sudo tar xzf actions-runner-linux-x64-2.329.0.tar.gz

# 4. Tạo user runner
sudo useradd -m runner

# 5. Chuyển quyền sở hữu
sudo chown -R runner:runner /opt/actions-runner

# 6. Cấu hình runner với URL repo và token
sudo -u runner /opt/actions-runner/config.sh --url https://github.com/vamnhcorder8/VPS --token B2KR5SVNDV67SF7WLVM46N3JE2OAY

# 7. Tạo systemd service chạy 24/7
sudo bash -c 'cat <<EOF >/etc/systemd/system/actions-runner.service
[Unit]
Description=GitHub Actions Runner
After=network.target
[Service]
ExecStart=/opt/actions-runner/run.sh
User=runner
WorkingDirectory=/opt/actions-runner
Restart=always
[Install]
WantedBy=multi-user.target
EOF'

# 8. Reload systemd và kích hoạt service
sudo systemctl daemon-reload
sudo systemctl enable actions-runner
sudo systemctl start actions-runner

# 9. Kiểm tra trạng thái và log realtime
sudo systemctl status actions-runner
sudo journalctl -u actions-runner -f

# 10. Quản lý service
sudo systemctl stop actions-runner
sudo systemctl restart actions-runner
