```
version: "3.9"

services:
  db:
    image: mariadb:11
    container_name: nextcloud-db-1
    restart: always
    volumes:
      - ./db_data:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=root
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud
      - MYSQL_PASSWORD=password

  app:
    image: nextcloud
    container_name: nextcloud-app-1
    restart: always
    depends_on:
      - db
    ports:
      - "8080:80"
    volumes:
      - ./nextcloud_data:/var/www/html
      - /mnt/external-storage:/mnt/external-storage
    environment:
      # DB接続設定（dbサービスと必ず一致させる）
      - MYSQL_HOST=db
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud
      - MYSQL_PASSWORD=password

      # 初回セットアップ自動化（お好みで値変えてOK）
      - NEXTCLOUD_ADMIN_USER=admin
      - NEXTCLOUD_ADMIN_PASSWORD=adminpass

      # trusted domains もここである程度入れておくと楽
      # 自分のLAN IPに合わせて適宜修正
      - NEXTCLOUD_TRUSTED_DOMAINS=localhost:8080 192.168.11.50:8080

volumes: 
  db_data:
  nextcloud_data:
```
