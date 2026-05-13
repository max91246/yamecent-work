# Yamecent Work

Yamecent 專案群的指揮中心與環境配置倉庫。

## 專案結構

```
yamecent-work/          ← 本倉庫（環境配置 + 指令中心）
yamecent-admin/         ← Laravel 9 API 後端 + 後台管理
yamecent-vue/           ← Vue 3 + Vite 前端 SPA
```

## 本地開發環境

所有服務統一由 `environment/` 管理，一個指令啟動全部：

```bash
cd environment
docker compose up -d
```

### 服務一覽

| 容器 | 說明 | 本地網址 |
|------|------|----------|
| yamecent-nginx | 統籌反向代理 | port 80 |
| yamecent-php-fpm | Laravel PHP 8.0 | — |
| yamecent-mysql | MySQL 8.4 | localhost:3306 |
| yamecent-redis | Redis 8 | localhost:6379 |
| yamecent-vite | pnpm build（production build only） | — |
| yamecent-flaresolverr | FlareSolverr 爬蟲輔助 | localhost:8191 |

### 本地網域（需加入 hosts）

```
127.0.0.1  yamecent-admin.local
127.0.0.1  yamecent-vue
```

| 網址 | 說明 |
|------|------|
| http://yamecent-admin.local | Laravel API 後端 |
| http://yamecent-vue | pure-admin v7 後台前端（serve dist/） |

## 目錄說明

```
environment/
├── docker-compose.yml          # 所有服務定義
├── docker-compose.override.yml # 本地 port 映射（自動套用）
└── docker/nginx/
    ├── nginx.conf
    ├── conf.d/                 # resolver 設定
    ├── sites-available/        # GCP 生產環境 nginx 設定
    └── sites-available-local/  # 本地開發 nginx 設定
        ├── yamecent-admin.conf
        └── yamecent-vue.conf
```

## GCP 生產環境部署

生產環境由各子專案的 docker-compose 各自管理：

```bash
# 在 GCP server 上
cd /home/max91246/www/yamecent-admin
sudo -u max91246 git pull
docker compose -f docker-compose.yml -f docker-compose.prod.yml up -d
```

## 相關倉庫

| 倉庫 | 說明 |
|------|------|
| [yamecent-admin](https://github.com/max91246/yamecent-admin) | Laravel 後端 |
| [yamecent-vue](https://github.com/max91246/yamecent-vue) | Vue 前端 |
| [yamecent-work](https://github.com/max91246/yamecent-work) | 本倉庫 |
