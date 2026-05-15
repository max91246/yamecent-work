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

各服務本地網域請參考 `environment/docker/nginx/sites-available-local/` 設定檔。

| 說明 | Port |
|------|------|
| Laravel API 後端 | 80 |
| pure-admin v7 後台前端（serve dist/） | 80 |

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

## GCP 生產環境

- Server：`[GCP_HOST]`（user: `[GCP_USER]`，key: `~/.ssh/id_rsa`）
- 網域：`[DOMAIN]` / `admin.[DOMAIN]`
- SSL：Let's Encrypt（90天自動更新）

> 實際連線資訊存於 GitHub Secrets，請向專案負責人取得。

### 容器一覽

| 容器 | Image | Port |
|------|-------|------|
| yamecent-nginx | nginx:1.25-alpine | 80 / 443 |
| yamecent-php-fpm | yamecent-admin-php-fpm | 9000 |
| yamecent-mysql | mysql:8.4 | 3306 |
| yamecent-redis | redis:8-alpine | 6379 |
| yamecent-flaresolverr | flaresolverr:latest | 8191 |

### CI/CD 部署流程

升級採 **手動觸發** 模式，開發者決定何時部署到線上。

```text
本地開發完成
    ↓
git push（admin: main / vue: master）
    ↓
前往 GitHub Actions 手動觸發
  Admin repo → Actions → Deploy Admin → Run workflow
  Vue   repo → Actions → Deploy Vue   → Run workflow
    ↓
Telegram 通知部署結果（成功 ✅ / 失敗 ❌）
```

**Admin 部署步驟：**
```bash
git pull origin main
composer dump-autoload --optimize
php artisan migrate --force
php artisan cache:clear
```

**Vue 部署步驟：**
```bash
git fetch origin master
git reset --hard origin/master
# nginx 自動 serve 新 dist/，無需 reload
```

### 手動 SSH 操作

```bash
ssh [GCP_USER]@[GCP_HOST]
docker exec yamecent-php-fpm php artisan [command]
```

## 相關倉庫

| 倉庫 | 說明 |
|------|------|
| yamecent-admin | Laravel 後端 |
| yamecent-vue | Vue 前端 |
| yamecent-work | 本倉庫（環境配置） |
