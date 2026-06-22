# 部署到 Hugging Face Spaces 指南

本项目已经配置了自动化部署到 **Hugging Face Spaces** 的 GitHub Actions 工作流。由于 Hugging Face 提供了免费且支持本地存储（持久化文件系统）的 Docker 环境，非常适合运行 `9router`。

## 部署步骤

1. **注册并创建 Space**：
   - 前往 [Hugging Face](https://huggingface.co/) 注册账号。
   - 创建一个新的 Space，环境选择 **Docker -> Blank**，硬件选择 **Free**。

2. **获取访问令牌 (Access Token)**：
   - 在 Hugging Face 的 Settings -> Access Tokens 中，创建一个具备对您该 Space **写入权限 (Write access)** 的 Fine-grained Token（或者传统 Write Token）。
   - 复制生成的 `hf_...` 令牌备用。

3. **配置 GitHub Secrets**：
   - 前往您的 GitHub 仓库的 **Settings -> Secrets and variables -> Actions**。
   - 添加以下三个 Secret：
     - `HF_TOKEN`：刚刚在步骤 2 中复制的令牌。
     - `HF_USERNAME`：您的 Hugging Face 用户名（例如：`xiaoming`）。
     - `HF_SPACE_NAME`：您在步骤 1 中创建的 Space 名称（例如：`9router`）。

4. **处理端口配置 (重要！)**：
   - `9router` 默认运行在 `20128` 端口，而 Hugging Face Spaces 默认监听 `7860` 端口。
   - 为了确保 Hugging Face 正常检测到服务运行，请转到您的 Hugging Face Space 的 **Settings** 页面。
   - 找到 **Variables and secrets** 部分。
   - 新增一个 Public Variable：
     - Name: `PORT`
     - Value: `7860`
   - 新增一个 Public Variable (可选，为了云端兼容性)：
     - Name: `NEXT_PUBLIC_BASE_URL`
     - Value: `https://您的用户名-您的space名称.hf.space`

5. **触发部署**：
   - 完成配置后，只需要将代码 `git push` 到 GitHub 的 `main` 分支。
   - GitHub Actions 会自动触发 `.github/workflows/deploy-hf.yml`，并将代码推送到 Hugging Face 进行自动构建。
   - 您可以前往 Hugging Face Space 页面查看 `Building` 进度，完成后即可使用！
