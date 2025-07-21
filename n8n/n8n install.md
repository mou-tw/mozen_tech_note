# 1. mkcert創建本地部屬憑證
n8n想要使用本地部屬，並且使用本地簽發的簽證進行ssl認證，可透過mkcert進行
一次性安裝本地的憑證信任根憑證
這步只需要做一次，它會在你的作業系統中建立並信任一個 CA（用來簽發你自己的憑證）：
```
mkcert -install
```

建立ssl憑證
```
mkcert hostname
```

# 2. docker 運行n8n
使用docker 安裝n8n，後使用docker 運行n8n
```
docker run -d --restart unless-stopped -it \
--name n8n \
-p 5678:5678 \
-e N8N_HOST="hostname.com" \
-e WEBHOOK_TUNNEL_URL="https://hostname.com" \
-e WEBHOOK_URL="https://hostname.com" \
-e N8N_SSL_KEY="/keypath" \
-e N8N_SSL_CERT="/certpath" \
-v n8n_data:/home/node/.n8n \
docker.n8n.io/n8nio/n8n
```


# 3. 透過nginx進行反向代理
安裝
```
sudo apt install nginx
```
設定nginx
```
sudo nano /etc/nginx/sites-available/n8n.conf
```
貼入參數
```
server {
    listen 443 ssl;
    server_name <server host name>;

    ssl_certificate cert_path;
    ssl_certificate_key key_path;

    location / {
        proxy_pass http://localhost:5678;
        proxy_http_version 1.1;

        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        
        # WebSocket specific settings
        proxy_read_timeout 86400;
        proxy_send_timeout 86400;

        chunked_transfer_encoding off;
        proxy_buffering off;
        proxy_cache off;
    }
}

```

soft link
```
sudo ln -s /etc/nginx/sites-available/n8n.conf /etc/nginx/sites-enabled/n8n.conf
```
Run the configuration test
```
sudo nginx -t
```
restart nginx
```
sudo systemctl restart nginx
```

測試nginx代理運作狀況
```
curl <hostname>
```

# 安裝rootCA到其他本地設備
此時已經有nginx已經成功代理，並且能夠訪問n8n，但如果是使用其他設備訪問仍會chrome憑證無法信任的問題，此時需要使用把server的rootCA給其他設備安裝。
過程必須要把整個憑證資料夾搬去其他設備中，重新跑一次mkcert install
查詢mkcert的rootCA位置
```
mkcert -CAROOT
```
copy folder
```
scp -r mozen@<IP>:<rootCA path> <local path>
```

在其他設備安裝一次
```
set CAROOT=<rootCA folder path>
mkcert -install
```

重啟chrome，就可以避免不信任設備的警告