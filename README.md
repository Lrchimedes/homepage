# 个人主页项目

这是一个基于HTML5、CSS3和JavaScript的单页面个人主页项目，具有现代化的设计风格和良好的交互体验。

## 项目结构

```
homepage/
├── static/
│   ├── css/              # 样式文件
│   ├── fonts/            # 字体文件
│   ├── img/              # 图片资源
│   └── js/               # JavaScript文件
├── index.html            # 主页面
├── .gitignore            # Git忽略文件
└── README.md             # 项目说明文档
```

## 部署到Cloudflare Pages

### 准备工作

1. 确保您已经有一个GitHub或GitLab账号
2. 确保您已经有一个Cloudflare账号

### 步骤1：将代码推送到GitHub/GitLab

#### GitHub

1. 在GitHub上创建一个新的仓库
2. 将本地仓库与GitHub仓库关联：
   ```bash
   git remote add origin https://github.com/your-username/your-repo-name.git
   ```
3. 将代码推送到GitHub：
   ```bash
   git push -u origin master
   ```

#### GitLab

1. 在GitLab上创建一个新的仓库
2. 将本地仓库与GitLab仓库关联：
   ```bash
   git remote add origin https://gitlab.com/your-username/your-repo-name.git
   ```
3. 将代码推送到GitLab：
   ```bash
   git push -u origin master
   ```

### 步骤2：部署到Cloudflare Pages

1. 登录[Cloudflare控制台](https://dash.cloudflare.com/)
2. 在左侧导航栏中选择**Pages**
3. 点击**创建项目**按钮
4. 选择**连接到Git**
5. 授权Cloudflare访问您的GitHub/GitLab账号
6. 选择您刚刚创建的仓库
7. 点击**开始设置**
8. 在**构建设置**中：
   - 生产分支：`master`（或您使用的主分支名称）
   - 构建命令：`（留空）`（因为这是静态网站，不需要构建）
   - 构建输出目录：`（留空）`（或`.）
   - 根目录：`（留空）`
9. 点击**保存并部署**
10. 等待部署完成，Cloudflare将为您生成一个域名

### 访问您的网站

部署完成后，您可以通过Cloudflare生成的域名访问您的网站，例如：`https://your-project-name.pages.dev`

您还可以添加自定义域名，方法是：
1. 在Pages项目设置中选择**自定义域名**
2. 输入您的自定义域名
3. 按照提示在您的域名注册商处添加DNS记录

## 部署到Cloudflare Workers

### 准备工作

1. 确保您已经安装了Node.js
2. 确保您已经有一个Cloudflare账号

### 步骤1：安装Wrangler CLI

Wrangler是Cloudflare Workers的命令行工具，用于部署和管理Workers项目。

```bash
npm install -g wrangler
```

### 步骤2：登录Wrangler

```bash
wrangler login
```

按照提示在浏览器中授权Wrangler访问您的Cloudflare账号。

### 步骤3：初始化Workers项目

在项目根目录下执行以下命令：

```bash
wrangler init
```

按照提示进行配置：
- 项目名称：使用默认名称或自定义
- 选择"Hello World"脚本
- 是否使用TypeScript：根据您的偏好选择
- 是否创建GitHub Actions：根据您的偏好选择

### 步骤4：配置wrangler.toml

编辑生成的`wrangler.toml`文件，配置您的Workers项目：

```toml
name = "your-workers-name"
main = "src/index.js"
compatibility_date = "2023-01-01"

[site]
bucket = "."
```

### 步骤5：编写Workers脚本

编辑`src/index.js`文件，编写一个简单的Workers脚本，用于提供静态文件服务：

```javascript
import { getAssetFromKV } from '@cloudflare/kv-asset-handler';

addEventListener('fetch', event => {
  event.respondWith(handleEvent(event));
});

async function handleEvent(event) {
  try {
    const response = await getAssetFromKV(event);
    return response;
  } catch (e) {
    let pathname = new URL(event.request.url).pathname;
    return new Response(`File not found: ${pathname}`, {
      status: 404,
    });
  }
}
```

### 步骤6：部署Workers

```bash
wrangler publish
```

部署完成后，Wrangler将为您生成一个域名，例如：`https://your-workers-name.your-username.workers.dev`

### 访问您的网站

您可以通过Wrangler生成的域名访问您的网站，例如：`https://your-workers-name.your-username.workers.dev`

您还可以添加自定义域名，方法是：
1. 在Cloudflare控制台中选择您的域名
2. 在左侧导航栏中选择**Workers Routes**
3. 点击**添加路由**
4. 输入您的自定义域名，例如：`example.com/*`
5. 选择您刚刚部署的Workers
6. 点击**保存**

## 总结

本项目可以通过两种方式部署到Cloudflare：

1. **Cloudflare Pages**：适合静态网站，部署简单，不需要编写额外的脚本
2. **Cloudflare Workers**：适合需要更多自定义逻辑的场景，需要编写Workers脚本

两种方式都提供了免费的使用额度，您可以根据自己的需求选择合适的部署方式。