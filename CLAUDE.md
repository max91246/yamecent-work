# Yamecent 專案群

這是 Yamecent 所有專案的指揮中心。從這裡下指令，Claude 會跨專案完成任務。

## 專案一覽

| 專案 | 路徑 | 技術 | 說明 |
| --- | --- | --- | --- |
| yamecent-admin | ../yamecent-admin | Laravel 9 + MySQL + Redis | API 後端 + 後台管理 |
| yamecent-vue | ../yamecent-vue | Vue 3 + Vite + Element Plus | 前端 SPA |

## 專案關係

```
yamecent-vue  ──/api/*──►  yamecent-admin（Laravel）
                               │
                           MySQL + Redis（GCP）
```

- Vue 所有 API 請求走 `/api/*`
- 本地：Vite proxy 轉發到 `http://yamecent-admin.local`
- 生產：同一台 GCP nginx，`/api/*` 轉給 php-fpm
- 認證：JWT（`Authorization: Bearer <token>`）

## GCP 環境

- Server：`34.122.76.154`（user: `user`，key: `~/.ssh/id_rsa`）
- 網域：`yamnews.net`
- 部署目錄：`/home/max91246/www/yamecent-admin`
- Docker Compose：`docker-compose.yml` + `docker-compose.prod.yml`

## 本地開發

| 服務 | 網址 |
| --- | --- |
| Laravel API | http://yamecent-admin.local |
| Vue Dev | http://localhost:5174 |
| MySQL | localhost:3306 |
| Redis | localhost:6379 |

## 規範

- 所有 GCP 變更走 git 流程（本地改 → push → GCP pull），禁止直接 SSH 改檔
- SQL 查詢使用 `yamecent.ya_*` 完整前綴
- 各專案詳細規範見各自 CLAUDE.md

## 各專案詳細規範

- [yamecent-admin](../yamecent-admin/CLAUDE.md)
- [yamecent-vue](../yamecent-vue/CLAUDE.md)
