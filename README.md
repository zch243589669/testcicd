# Test CICD Vue

🚀 Vue 3 测试项目 - 配合 GitHub Actions + 阿里云 ACR 实现 CI/CD

## 技术栈

- **Vue 3** - 前端框架
- **GitHub Actions** - CI/CD 流水线
- **Alibaba Cloud ACR** - 容器镜像服务
- **Docker** - 容器化部署
- **Nginx** - Web 服务器

## 快速开始

### 本地开发

```bash
# 安装依赖
npm install

# 启动开发服务器
npm run serve

# 构建生产版本
npm run build
```

### Docker 本地测试

```bash
# 构建镜像
docker build -t test-cicd-vue .

# 运行容器
docker run -d -p 8080:80 --name vue-app test-cicd-vue

# 访问 http://localhost:8080
```

## CI/CD 流程

```
Git Push → GitHub Actions → Build → Push to ACR → Deploy
```

## 配置说明

### 1. ACR 准备

- 登录 [阿里云容器镜像服务](https://cr.console.aliyun.com/)
- 创建命名空间（如 `your-namespace`）
- 记录 Registry 地址（如 `registry.cn-hangzhou.aliyuncs.com`）

### 2. GitHub Secrets 设置

在仓库 Settings → Secrets → Actions 中添加：

| Secret | 说明 |
|--------|------|
| `ACR_USERNAME` | 阿里云账号用户名 |
| `ACR_PASSWORD` | 阿里云账号密码或固定令牌 |

### 3. 修改配置

编辑 `.github/workflows/build-and-push.yml`，将 `NAMESPACE` 改为你的命名空间：

```yaml
env:
  NAMESPACE: your-namespace  # ← 修改这里
```

### 4. 推送触发

```bash
git add .
git commit -m "Initial commit"
git push origin main
```

## 部署到 ECS

```bash
# 登录 ACR
docker login --username=<用户名> registry.cn-hangzhou.aliyuncs.com

# 拉取镜像
docker pull registry.cn-hangzhou.aliyuncs.com/<命名空间>/test-cicd-vue:latest

# 运行
docker run -d \
  --name vue-app \
  -p 80:80 \
  --restart always \
  registry.cn-hangzhou.aliyuncs.com/<命名空间>/test-cicd-vue:latest
```

## 项目结构

```
.
├── .github/
│   └── workflows/
│       └── build-and-push.yml    # GitHub Actions 配置
├── public/
│   └── index.html                # HTML 模板
├── src/
│   ├── App.vue                   # 主组件
│   └── main.js                   # 入口文件
├── Dockerfile                    # Docker 构建配置
├── nginx.conf                    # Nginx 配置
├── package.json                  # 项目依赖
├── vue.config.js                 # Vue CLI 配置
└── README.md                     # 本文件
```

## License

MIT
