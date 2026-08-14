# MacCMS VPS 运维信息汇总

> 生成时间：2026-08-12 11:16（Asia/Singapore）  
> 凭证补全时间：2026-08-12（Asia/Singapore）  
> VPS 时间：2026-08-12 03:16（UTC）  
> 用途：记录当前 `cms.mmeme.me` 在 RackNerd VPS 上的部署、目录、服务、采集与维护信息。

> [!warning] 敏感文档
> 本文档现在包含 MacCMS 后台密码和 MariaDB 密码。不要提交到 Git，不要发布到公开网盘或分享给无关人员。

## 1. 快速索引

| 项目             | 地址或路径                                                             |
| -------------- | ----------------------------------------------------------------- |
| 网站             | https://cms.mmeme.me/                                             |
| 后台登录           | https://cms.mmeme.me/panel_df3e85cb.php/admin/index/index.html    |
| Mxone Pro 设置   | https://cms.mmeme.me/panel_df3e85cb.php/admin/mxpro/mxproset.html |
| VPS 公网 IP      | `107.173.89.101`                                                  |
| 本机 SSH 命令      | `ssh vps-107`                                                     |
| MacCMS 根目录     | `/var/www/maccms10`                                               |
| 当前前台模板         | `/var/www/maccms10/template/mxpro`                                |
| Mxone Pro 静态资源 | `/var/www/maccms10/mxtheme`                                       |
| 自动采集脚本         | `/usr/local/sbin/maccms-collect.php`                              |
| 自动采集 cron      | `/etc/cron.d/maccms-collect`                                      |
| 自动采集日志         | `/var/log/maccms-collect.log`                                     |
| 数据库配置          | `/var/www/maccms10/application/database.php`                      |
| 统一凭证记录         | `/root/maccms-credentials.txt`                                    |
| MariaDB 实际数据目录 | `/var/lib/mysql/maccms`                                           |
| Nginx 站点配置     | `/etc/nginx/sites-available/maccms10`                             |
| HTTPS 证书入口     | `/etc/letsencrypt/live/cms.mmeme.me/`                             |

## 2. 服务器概况

| 项目 | 当前值 |
| --- | --- |
| 主机名 | `racknerd-225f55a` |
| 操作系统 | Ubuntu 24.04.3 LTS |
| 内核 | `6.8.0-136-generic` |
| CPU | 2 核 |
| 内存 | 2.4 GiB，总可用约 1.8 GiB（采集时会波动） |
| 系统盘 | 43 GiB，已用约 19 GiB，剩余约 23 GiB |
| VPS 时区 | `Etc/UTC` |
| 公网地址 | `107.173.89.101` |
| 内网地址 | `10.0.0.1` |

当前对外端口：

- SSH：`22/tcp`
- HTTP：`80/tcp`
- HTTPS：`443/tcp`
- MariaDB：`127.0.0.1:3306`，仅监听本机，不直接暴露公网

UFW 防火墙已启用，只放行：

- `OpenSSH`
- `Nginx Full`

## 3. SSH 信息

本机 SSH 配置：

```sshconfig
Host vps-107 107.173.89.101
  HostName 107.173.89.101
  User root
  Port 22
  IdentityFile ~/.ssh/id_ed25519
  IdentitiesOnly yes
  AddKeysToAgent yes
```

相关位置：

| 用途 | 路径 | 权限 |
| --- | --- | --- |
| 本机 SSH 配置 | `/Users/wangwenbo/.ssh/config` | — |
| 本机私钥 | `/Users/wangwenbo/.ssh/id_ed25519` | `600` |
| VPS 已授权公钥 | `/root/.ssh/authorized_keys` | `600` |

不要把私钥内容上传到 VPS、Git 仓库或公开网盘。服务器 root 密码未写入本文档；日常运维应使用 SSH key。

## 4. 网站入口

### MacCMS 后台管理凭证

| 项目 | 当前值 |
| --- | --- |
| 后台入口 | `https://cms.mmeme.me/panel_df3e85cb.php` |
| 后台用户名 | `admin` |
| 后台密码 | `8a2a9537c5d6853012` |
| VPS 上的统一凭证文件 | `/root/maccms-credentials.txt` |

后台入口文件名和密码都应当作敏感信息。如果修改后台密码，记得同步更新本文档和 `/root/maccms-credentials.txt`。

| 用途 | 路径 |
| --- | --- |
| 前台入口 | `/var/www/maccms10/index.php` |
| API 入口 | `/var/www/maccms10/api.php` |
| 隐藏后台入口 | `/var/www/maccms10/panel_df3e85cb.php` |
| 安装锁 | `/var/www/maccms10/application/data/install/install.lock` |

当前验证结果：

- `http://cms.mmeme.me/` 自动 301 跳转至 HTTPS。
- `https://cms.mmeme.me/` 返回 HTTP 200。
- `https://cms.mmeme.me/install.php` 返回 HTTP 404。
- 后台和模板设置地址在未登录状态下会 302 跳转到后台登录页。
- 安装锁权限为 `444`，并且 Nginx 单独禁止外部访问 `install.php`。

## 5. MacCMS 根目录结构

MacCMS 根目录：

```text
/var/www/maccms10
├── index.php                    # 前台入口
├── api.php                      # API 入口
├── panel_df3e85cb.php           # 后台入口
├── application/                 # MacCMS 业务代码和配置
│   ├── admin/                   # 后台控制器及后台模板
│   ├── api/                     # API 控制器
│   ├── command/                 # CLI 命令
│   ├── common/                  # 公共控制器、模型、行为、验证器
│   ├── data/                    # 安装锁、更新数据、备份数据等
│   ├── extra/                   # 主要运行配置
│   ├── index/                   # 前台控制器
│   ├── install/                 # 安装程序代码（外部入口已禁用）
│   └── lang/                    # 语言包
├── addons/                      # 插件
├── extend/                      # 扩展库
├── static/                      # 原版静态资源及播放器配置
├── static_new/                  # 当前分支新增后台/静态资源
├── template/
│   ├── default/                 # 原始默认模板，仍保留
│   └── mxpro/                   # 当前启用的 Mxone Pro 模板
├── mxtheme/                     # Mxone Pro 的 CSS、JS、图片、字体
├── upload/                      # 图片和附件上传目录
├── runtime/                     # 缓存、编译模板、监控临时数据
├── vendor/                      # Composer 依赖
├── thinkphp/                    # ThinkPHP 框架
├── tests/                       # 测试文件
└── .git/                        # Git 仓库元数据
```

主要目录当前大小：

| 目录 | 大小 |
| --- | ---: |
| `/var/www/maccms10` | 约 122 MiB |
| `/var/www/maccms10/application` | 约 12 MiB |
| `/var/www/maccms10/template` | 约 21 MiB |
| `/var/www/maccms10/template/default` | 约 20 MiB |
| `/var/www/maccms10/template/mxpro` | 约 680 KiB |
| `/var/www/maccms10/mxtheme` | 约 3.1 MiB |
| `/var/www/maccms10/runtime` | 约 28 MiB |
| `/var/www/maccms10/upload` | 约 80 KiB |
| `/var/www/maccms10/static` | 约 8 MiB |
| `/var/www/maccms10/static_new` | 约 14 MiB |

## 6. 关键 MacCMS 配置

### 核心配置目录

```text
/var/www/maccms10/application/extra/
├── addons.php
├── bind.php                    # 采集分类绑定
├── blacks.php
├── captcha.php
├── domain.php
├── maccms.php                  # 站点核心配置、模板选择
├── mctheme.php
├── mxprost.php                 # Mxone Pro 模板设置
├── queue.php
├── quickmenu.php
├── timming.php
├── type_synonyms.php
├── version.php
├── voddowner.php
├── vodplayer.php               # 播放器线路配置
└── vodserver.php
```

关键文件：

| 用途 | 路径 |
| --- | --- |
| 站点和模板选择 | `/var/www/maccms10/application/extra/maccms.php` |
| 数据库连接 | `/var/www/maccms10/application/database.php` |
| 采集分类绑定 | `/var/www/maccms10/application/extra/bind.php` |
| 播放器配置 | `/var/www/maccms10/application/extra/vodplayer.php` |
| 定时任务相关配置 | `/var/www/maccms10/application/extra/timming.php` |
| Mxone Pro 配置 | `/var/www/maccms10/application/extra/mxprost.php` |

当前模板配置：

```text
template_dir=mxpro
mob_template_dir=mxpro
```

注意：`maccms.php` 内的 `site_url` 和 `site_wapurl` 仍是安装默认值 `www.test.cn`、`wap.test.cn`；当前实际域名由 Nginx 和访问请求确定，为 `cms.mmeme.me`。如以后启用依赖固定站点域名的生成任务，应在后台把这两项改成实际域名。

## 7. Mxone Pro 模板

| 用途 | 路径 |
| --- | --- |
| 模板 HTML | `/var/www/maccms10/template/mxpro/html` |
| 模板广告 | `/var/www/maccms10/template/mxpro/ads` |
| 模板自带后台资源 | `/var/www/maccms10/template/mxpro/asset` |
| CSS | `/var/www/maccms10/mxtheme/css` |
| JavaScript | `/var/www/maccms10/mxtheme/js` |
| 图片 | `/var/www/maccms10/mxtheme/images` |
| 字体 | `/var/www/maccms10/mxtheme/fonts` |
| 模板后台控制器 | `/var/www/maccms10/application/admin/controller/Mxpro.php` |
| 模板后台页面 | `/var/www/maccms10/application/admin/view_new/system/mxprocms.html` |
| 模板设置数据 | `/var/www/maccms10/application/extra/mxprost.php` |

模板管理页面：

```text
https://cms.mmeme.me/panel_df3e85cb.php/admin/mxpro/mxproset.html
```

## 8. 自动采集任务

### 文件位置

| 用途 | 路径 |
| --- | --- |
| 主采集脚本 | `/usr/local/sbin/maccms-collect.php` |
| cron 定时任务 | `/etc/cron.d/maccms-collect` |
| 执行日志 | `/var/log/maccms-collect.log` |
| 防重复执行锁 | `/run/maccms-collect.lock` |

采集脚本权限为 `700`，只有 root 可以读取和执行。

### 当前 cron

```cron
17 */6 * * * root flock -n /run/maccms-collect.lock /usr/bin/php /usr/local/sbin/maccms-collect.php --hours=30 >> /var/log/maccms-collect.log 2>&1
```

解释：

- 每 6 小时的第 17 分钟执行。
- VPS 使用 UTC，因此执行时间是 UTC `00:17 / 06:17 / 12:17 / 18:17`。
- 换算为新加坡/北京时间是 `08:17 / 14:17 / 20:17 / 次日 02:17`。
- `--hours=30` 表示采集资源方最近 30 小时的数据。
- `flock` 保证上一次任务没结束时，不重复启动第二个采集进程。
- 标准输出和错误都追加到 `/var/log/maccms-collect.log`。

### 手动运行

正常执行最近 30 小时采集：

```bash
ssh vps-107
/usr/bin/php /usr/local/sbin/maccms-collect.php --hours=30
```

只更新资源站、分类绑定和播放器配置，不拉取数据：

```bash
/usr/bin/php /usr/local/sbin/maccms-collect.php --setup-only
```

测试时限制采集页数：

```bash
/usr/bin/php /usr/local/sbin/maccms-collect.php --hours=30 --max-pages=2
```

### 当前资源站

| 名称 | API |
| --- | --- |
| 量子资源 | `https://cj.lziapi.com/api.php/provide/vod/` |
| 非凡资源 | `https://cj.ffzyapi.com/api.php/provide/vod/` |
| 光速资源 | `https://api.guangsuapi.com/api.php/provide/vod/` |
| 红牛资源 | `https://www.hongniuzy2.com/api.php/provide/vod/` |
| 暴风资源 | `https://bfzyapi.com/api.php/provide/vod/` |

这些资源站也记录在数据库表 `mac_collect` 中。采集脚本里的来源配置是自动任务的实际来源；如果只在后台修改资源站，而不修改脚本，下一次 `--setup-only` 或自动采集可能按脚本配置重新同步。

### 一次性四站全量采集队列（2026-08-13）

为量子、非凡、光速、红牛分别部署了独立的全量脚本，并由 systemd 严格串行执行：

| 资源站 | 全量脚本 | 日志 |
| --- | --- | --- |
| 量子资源 | `/usr/local/sbin/maccms-full-liangzi.sh` | `/var/log/maccms-full-liangzi.log` |
| 非凡资源 | `/usr/local/sbin/maccms-full-feifan.sh` | `/var/log/maccms-full-feifan.log` |
| 光速资源 | `/usr/local/sbin/maccms-full-guangsu.sh` | `/var/log/maccms-full-guangsu.log` |
| 红牛资源 | `/usr/local/sbin/maccms-full-hongniu.sh` | `/var/log/maccms-full-hongniu.log` |

队列管理：

| 用途 | 路径 |
| --- | --- |
| 串行队列脚本 | `/usr/local/sbin/maccms-full-queue.sh` |
| systemd 服务 | `/etc/systemd/system/maccms-full-queue.service` |
| 队列总日志 | `/var/log/maccms-full-queue.log` |
| 全量前数据库备份 | `/root/maccms-before-four-full-20260813-020731.sql.gz` |

队列顺序为：量子 → 非凡 → 光速 → 红牛。整个队列持有 `/run/maccms-collect.lock`，因此在全量任务运行期间，每 6 小时的增量 cron 会自动跳过，不会并发写入。systemd 为队列设置了较低 CPU/I/O 优先级。

查看当前状态和进度：

```bash
systemctl status maccms-full-queue.service
tail -f /var/log/maccms-full-queue.log
tail -f /var/log/maccms-full-liangzi.log
```

队列正常完成后，`maccms-full-queue.service` 会变为 `inactive (dead)` 且 `Result=success`。这是一次性任务，服务没有 enable 为开机自动重跑。

单站脚本也可用 `--max-pages=2` 做小范围测试：

```bash
/usr/local/sbin/maccms-full-liangzi.sh --max-pages=2
```

注意：这些全量脚本不应在队列正在运行时另行启动。卧龙资源的旧接口已失效，已从后台删除，也未进入自动脚本。

查看最近采集结果：

```bash
tail -n 100 /var/log/maccms-collect.log
```

## 9. MariaDB 数据库

### 连接信息

| 项目 | 当前值 |
| --- | --- |
| 数据库类型 | MariaDB/MySQL |
| Host | `127.0.0.1` |
| Port | `3306` |
| Database | `maccms` |
| User | `maccms` |
| Password | `fa473a77a317069eeba8b3f4bbacab70` |
| 表前缀 | `mac_` |
| Charset | `utf8` |
| 密码保存位置 | `/var/www/maccms10/application/database.php` |
| 统一凭证记录 | `/root/maccms-credentials.txt` |

数据库凭证同时保存在 MacCMS 连接配置和 root 专用的统一凭证文件中。需要在 VPS 上核对时：

```bash
ssh vps-107
sudo less /root/maccms-credentials.txt
sudo less /var/www/maccms10/application/database.php
```

`/root/maccms-credentials.txt` 还记录了 `SITE_URL`、`ADMIN_URL`、`ADMIN_USER`、`ADMIN_PASSWORD`、`DB_HOST`、`DB_PORT`、`DB_NAME`、`DB_USER` 和 `DB_PASSWORD`。它只应允许 root 读取。

### 数据库实际存储位置

```text
/var/lib/mysql/maccms
```

MariaDB 不是 SQLite，不存在一个可随意复制的单一数据库文件。不要在 MariaDB 运行时直接复制 `/var/lib/mysql/maccms` 作为常规备份，应使用逻辑备份：

```bash
mariadb-dump --single-transaction --routines --triggers maccms | gzip -9 > /root/maccms-$(date +%F).sql.gz
```

当前数据库概况：

- 表数量：81
- 数据库逻辑大小：约 26.66 MiB
- 磁盘目录大小：约 31 MiB
- 影视数据：2869 条
- 分类数据：59 条
- 资源站表：`mac_collect`
- 主要影视表：`mac_vod`
- 分类表：`mac_type`

MariaDB 配置目录：

```text
/etc/mysql/
/etc/mysql/mariadb.conf.d/
```

## 10. Nginx

| 用途 | 路径 |
| --- | --- |
| 主配置 | `/etc/nginx/nginx.conf` |
| 站点配置 | `/etc/nginx/sites-available/maccms10` |
| 已启用链接 | `/etc/nginx/sites-enabled/maccms10` |
| 站点根目录 | `/var/www/maccms10` |
| 站点访问日志 | `/var/log/nginx/maccms10.access.log` |
| 站点错误日志 | `/var/log/nginx/maccms10.error.log` |

主要配置行为：

- `server_name cms.mmeme.me`
- HTTP 自动跳转 HTTPS
- PHP 转发到 `/run/php/php8.3-fpm.sock`
- 只允许 `index.php`、`api.php` 和隐藏后台 PHP 入口执行
- 明确拒绝其他任意 `.php` 文件
- 明确阻止访问 `application`、`thinkphp`、`vendor`、`extend`、`runtime` 等内部目录
- 明确禁止外部访问 `install.php`

修改配置后的检查与重载：

```bash
nginx -t
systemctl reload nginx
```

## 11. PHP-FPM

| 项目 | 当前值或路径 |
| --- | --- |
| PHP 版本 | 8.3.6 |
| FPM 服务 | `php8.3-fpm.service` |
| FPM 主配置 | `/etc/php/8.3/fpm/php.ini` |
| FPM Pool 配置 | `/etc/php/8.3/fpm/pool.d/www.conf` |
| FPM Socket | `/run/php/php8.3-fpm.sock` |
| CLI 配置 | `/etc/php/8.3/cli/php.ini` |

已安装的关键扩展包括：

```text
curl fileinfo gd intl mbstring mysqli mysqlnd openssl PDO pdo_mysql zip
```

注意：命令行采集脚本使用 CLI PHP 配置；网站请求使用 FPM PHP 配置，两份 `php.ini` 不是同一个文件。

## 12. HTTPS 和域名

| 项目 | 当前值或路径 |
| --- | --- |
| 域名 | `cms.mmeme.me` |
| A 记录目标 | `107.173.89.101` |
| 证书提供方 | Let's Encrypt |
| 证书入口 | `/etc/letsencrypt/live/cms.mmeme.me/fullchain.pem` |
| 私钥入口 | `/etc/letsencrypt/live/cms.mmeme.me/privkey.pem` |
| 续期配置 | `/etc/letsencrypt/renewal/cms.mmeme.me.conf` |
| 证书有效期 | 2026-08-11 至 2026-11-09 |
| 自动续期 | `certbot.timer` 已启用且正在运行 |

证书真实文件在 `/etc/letsencrypt/archive/cms.mmeme.me/`，`live` 目录内是由 Certbot 管理的软链接。不要手工替换这些链接。

## 13. 日志与缓存

| 用途 | 路径 |
| --- | --- |
| Nginx 站点访问日志 | `/var/log/nginx/maccms10.access.log` |
| Nginx 站点错误日志 | `/var/log/nginx/maccms10.error.log` |
| 自动采集日志 | `/var/log/maccms-collect.log` |
| MacCMS 运行时目录 | `/var/www/maccms10/runtime` |
| MacCMS 缓存 | `/var/www/maccms10/runtime/cache` |
| 编译模板/临时文件 | `/var/www/maccms10/runtime/temp` |
| 监控运行数据 | `/var/www/maccms10/runtime/monitor` |

常用查看命令：

```bash
tail -f /var/log/nginx/maccms10.error.log
tail -f /var/log/maccms-collect.log
journalctl -u php8.3-fpm -f
journalctl -u mariadb -f
```

如果只需要清理 MacCMS 编译缓存，应精确清理 `runtime/cache` 和 `runtime/temp`，不要删除整个站点目录。

## 14. 上传目录

上传文件根目录：

```text
/var/www/maccms10/upload
```

当前子目录包括：

```text
actor/ actor_editor/
art/ art_editor/ art_screenshot/
files/
role/ role_editor/
topic/ topic_editor/
user/
vod/ vod_editor/ vod_file/ vod_screenshot/ vodthumb/
website/ website_editor/ website_screenshot/
```

目前大部分海报仍使用资源站外链，因此本地 `upload` 目录只有约 80 KiB。

## 15. Git 状态

| 项目 | 当前值 |
| --- | --- |
| 仓库 | `https://github.com/magicblack/maccms10.git` |
| 分支 | `master` |
| 当前基础 commit | `8ef06828408694a5d63d9c27b772ea5520f7f3ee` |
| 工作区 | 非干净状态，约 159 项修改或未跟踪文件 |

Mxone Pro、隐藏后台入口、安装锁、采集相关播放器配置及其他服务器定制尚未形成一个新的 Git commit。

不要执行以下命令：

```bash
git reset --hard
git clean -fd
```

它们可能删除当前模板、后台入口和服务器定制。升级前应先做数据库和站点文件备份，再单独审查差异。

## 16. 当前保留的模板包

```text
/root/mxonepro-deployed-patched-20260812.tar.gz
```

- 大小：约 2.5 MiB
- 内容：修复后的 `mxpro` 模板、`mxtheme` 静态资源、模板后台控制器、后台视图和模板配置。
- 这不是完整站点备份，也不包含数据库。
- 之前的部署前完整备份已按要求删除。

## 17. 服务管理命令

查看状态：

```bash
systemctl status nginx
systemctl status php8.3-fpm
systemctl status mariadb
systemctl status cron
systemctl status ssh
```

安全重载 Web 服务：

```bash
nginx -t && systemctl reload nginx
systemctl reload php8.3-fpm
```

查看监听端口：

```bash
ss -lntp
```

查看磁盘与内存：

```bash
df -h
free -h
du -sh /var/www/maccms10 /var/lib/mysql/maccms
```

## 18. 推荐的完整备份方式

数据库：

```bash
mariadb-dump --single-transaction --routines --triggers maccms | gzip -9 > /root/maccms-$(date +%F).sql.gz
```

站点和服务配置：

```bash
tar -C / -czf /root/maccms-files-$(date +%F).tar.gz \
  var/www/maccms10 \
  etc/nginx/sites-available/maccms10 \
  etc/cron.d/maccms-collect \
  usr/local/sbin/maccms-collect.php \
  etc/letsencrypt/renewal/cms.mmeme.me.conf
```

备份后应生成校验值：

```bash
sha256sum /root/maccms-*.gz
```

证书私钥、数据库配置和数据库备份都包含敏感信息，不应提交到 Git 或放到公开下载目录。

## 19. 维护时最重要的路径

```text
/var/www/maccms10                                      MacCMS 根目录
/var/www/maccms10/application/extra/maccms.php         站点配置和模板选择
/var/www/maccms10/application/database.php             数据库凭证
/root/maccms-credentials.txt                            后台与数据库统一凭证记录（敏感）
/var/www/maccms10/application/extra/bind.php            分类绑定
/var/www/maccms10/application/extra/vodplayer.php       播放线路
/var/www/maccms10/application/extra/mxprost.php         Mxone Pro 设置
/var/www/maccms10/template/mxpro                        当前模板
/var/www/maccms10/mxtheme                              当前模板静态资源
/var/www/maccms10/upload                               本地上传文件
/var/www/maccms10/runtime                              缓存和临时数据
/usr/local/sbin/maccms-collect.php                     自动采集脚本
/etc/cron.d/maccms-collect                             自动采集计划
/var/log/maccms-collect.log                            自动采集日志
/etc/nginx/sites-available/maccms10                    Nginx 站点配置
/var/log/nginx/maccms10.access.log                     站点访问日志
/var/log/nginx/maccms10.error.log                      站点错误日志
/etc/php/8.3/fpm/php.ini                               PHP-FPM 配置
/etc/php/8.3/fpm/pool.d/www.conf                       PHP-FPM Pool 配置
/var/lib/mysql/maccms                                  MariaDB 实际数据目录
/etc/letsencrypt/live/cms.mmeme.me                     HTTPS 证书入口
/root/mxonepro-deployed-patched-20260812.tar.gz        当前修复模板包
```

---

本文档已包含 MacCMS 后台密码和数据库密码，但不包含 VPS root 密码、SSH 私钥正文或 HTTPS 私钥正文。请将整份文档按敏感运维资料管理。
