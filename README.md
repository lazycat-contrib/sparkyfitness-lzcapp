# SparkyFitness 懒猫微服包

自托管健身与营养追踪器：记录训练、饮食与身体指标，支持目标与报表。

- 上游：<https://github.com/CodeWithCJ/SparkyFitness>
- 镜像：`codewithcj/sparkyfitness` / `codewithcj/sparkyfitness_server`
- 许可：MIT（全文见 content/licenses/LICENSE；仓库根 LICENSE 无标题，故 GitHub 标 NOASSERTION）

## 组成

| 服务 | 镜像 | 作用 |
| --- | --- | --- |
| `frontend` | `codewithcj/sparkyfitness` | 前端 + nginx 反代，监听 80 |
| `server` | `codewithcj/sparkyfitness_server` | Node 后端，监听 3010 |
| `db` | `postgres:18.3-alpine` | 数据库 |

## 关键配置说明

- **`SPARKY_FITNESS_SERVER_HOST` 必须是后端服务名**：在上游 compose 的注释里明确写着，
  前端 nginx 容器内写 `localhost` 会指向自己，导致每次 API 调用返回 502。
- **注册不需要邮箱验证**：上游 `auth.ts` 中 `requireEmailVerification: false`
  且注释写明「no verification email is ever sent, and there is no verify route」，
  另有 `requireLocalEmailVerified: false`。因此未配 SMTP 也能正常建号。
- `BETTER_AUTH_SECRET` / `SPARKY_FITNESS_API_ENCRYPTION_KEY` 用稳定密钥，
  前者变化会让所有人被登出。
- PostgreSQL 18 的挂载点是 `/var/lib/postgresql`（不是旧版的 `.../data`）。
- 应用自带账号体系，因此开放 `public_path`，由应用自己鉴权。

## 未验证部分

未在真机上实测：注册登录、记录训练与饮食、报表展示、重启后数据留存。
