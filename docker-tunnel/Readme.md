# داکر تانل (SSH Tunnel Dockerize)


## پیش نیاز
### ubuntu (root)
```
apt install docker.io -y && install nginx -y
```
### Centos (root)
```
yum install docker.io -y && install nginx -y
```


## استفاده

با یک مثال ساده تر است. فرض کنید می‌خواهید برنامه شما درخواست‌های HTTP از اینترنت را روی پورت‌های معمولی 80 و 443 بپذیرد، و برنامه همچنین به پورت‌های 80 و 443 روی دستگاه توسعه‌دهنده شما گوش می‌دهد، بنابراین می‌توانید برنامه را با HTTP و هر دو تست کنید. درخواست های HTTPS مانند زمان تولید. برای راه‌اندازی تونل و نمایش برنامه‌تان، دو کانتینر لازم است. ابتدا روی سروری که در معرض اینترنت قرار دارد و داکر نصب شده است (سرور خارج) اجرا کنید:
```
docker run --name tunnel-proxy --env PORTS="80:3000,443:3001" -itd --net=host vitobotta/docker-tunnel:0.31.0 proxy
```

سپس، در سرور relay، اجرا کنید:
```
docker run --name tunnel-app --env PORTS="80:3000,443:3001" --env PROXY_HOST="1.2.3.4" --env PROXY_SSH_PORT="22" --env PROXY_SSH_USER="${USER} " -v "${HOME}/.ssh/id_rsa:/ssh.key" -itd vitobotta/docker-tunnel:0.31.0 app
```
یک آرگومان اختیاری APP_IP وجود دارد که به صورت پیش‌فرض روی IP میزبان Docker است، اما اگر درخواست‌ها باید به یک IP خاص ارسال شوند، می‌توان آن را پیکربندی کرد.

PROXY_HOST="1.2.3.4"
PORTS="80:3000,443:3001"
PROXY_SSH_PORT="22"


توجه داشته باشید که فرض بر این است که اتصال SSH از احراز هویت کلید استفاده می کند، بنابراین باید کلید SSH را که می خواهید استفاده کنید، همانطور که در دستور بالا نشان داده شده است سوار کنید.

"${HOME}/.ssh/id_rsa:/ssh.key"
