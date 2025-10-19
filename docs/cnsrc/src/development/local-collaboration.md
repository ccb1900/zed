# 本地协作指南

1. 确保您已获得我们云基础设施的访问权限。若暂无访问权限，当前将无法进行本地协作。

2. 请根据您的操作系统平台安装Zed所需依赖环境：

- [macOS系统](#macos)
- [Linux系统](#linux)
- [Windows系统](#backend-windows)

请注意：在Windows平台上，`collab`仅支持通过MSVC工具链进行编译

3. 克隆我们的云端代码库，并按照云端README文档的指引进行操作

4. 配置适用于您平台的本地数据库：

- [macOS与Linux系统](#database-unix)
- [Windows系统](#database-windows)

5. 启动协作功能：

- [macOS与Linux系统](#run-collab-unix)
- [Windows系统](#run-collab-windows)

## 后端依赖环境

若您正在开发Zed的协作功能，需要安装zed的`collab`服务器所需依赖：

- PostgreSQL数据库
- LiveKit实时通信框架
- Foreman进程管理工具

您可以选择原生安装这些依赖，或通过Docker容器环境运行。

### macOS系统

1. 安装 [Postgres.app](https://postgresapp.com) 或通过 [homebrew 安装 postgresql](https://formulae.brew.sh/formula/postgresql@15)：

   ```sh
   brew install postgresql@15
   ```

2. 安装 [Livekit](https://formulae.brew.sh/formula/livekit) 和 [Foreman](https://formulae.brew.sh/formula/foreman)

   ```sh
   brew install livekit foreman
   ```

- 按照 [collab README](https://github.com/zed-industries/zed/blob/main/crates/collab/README.md) 中的步骤配置 Postgres 数据库以进行集成测试

或者，如果您已安装 [Docker](https://www.docker.com/)，可以使用 Docker Compose 启动所有 `collab` 依赖项。

### Linux

1. 安装 [Postgres](https://www.postgresql.org/download/linux/)

[[代码块0]]

2. 安装 [Livekit](https://github.com/livekit/livekit-cli)

   [[代码块1]]

3. 安装 [Foreman](https://theforeman.org/manuals/3.15/quickstart_guide.html)

### Windows 系统 {#backend-windows}

> 本节内容仍在开发中，说明尚未完善。

- 安装 [Postgres](https://www.postgresql.org/download/windows/)
- 安装 [Livekit](https://github.com/livekit/livekit)，您可以选择将 [[代码块2]] 二进制文件添加到 [[代码块3]]。

另外，如果您已安装 [Docker](https://www.docker.com/)，可以使用 Docker Compose 启动所有 [[代码块4]] 依赖项。

### Docker 方式 {#Docker}

若您已安装 Docker 或 Podman，可通过 Docker Compose 在容器中运行后端依赖服务：

```sh
docker compose up -d
```

## 数据库配置

在本地运行 `collab` 服务器前，需先配置 `zed` PostgreSQL 数据库。

### macOS 与 Linux 系统 {#database-unix}

```sh
script/bootstrap
```

该脚本将创建 `zed` PostgreSQL 数据库并预置用户数据。由于需要从 GitHub API 获取用户信息，执行过程中需保持网络连接。

数据库初始内容由以下文件定义：

```sh
cat crates/collab/seed.default.json
```

如需使用不同的管理员用户组，您可以创建自定义 JSON 文件并通过 `SEED_PATH` 环境变量指定路径。请注意，管理员列表中的用户名目前必须对应有效的 GitHub 用户账户。

```json [settings]
{
  "admins": ["admin1", "admin2"],
  "channels": ["zed"]
}
```

### Windows 系统 {#database-windows}

## 本地测试协作功能

### 在 macOS 和 Linux 系统上 {#run-collab-unix}

确保已配置并运行 Postgres，然后启动 Zed 的协作服务器和 `livekit` 开发服务器：

```sh
foreman start
# OR
docker compose up
```

或者，如果您不需要测试语音和屏幕共享功能，可以仅运行 `collab` 和 `cloud`，无需启动 `livekit` 开发服务器：

```sh
cargo run -p collab -- serve all
```

```sh
cd ../cloud; cargo make dev
```

在新终端窗口中，运行两个或多个 Zed 实例：

```sh
script/zed-local -3
```

该脚本会根据 `-2`、`-3` 或 `-4` 参数启动一至四个 Zed 实例。每个实例都将连接到本地 `collab` 服务器，并以 `.admins.json` 或 `.admins.default.json` 中的不同用户身份登录。

### 在 Windows 系统上 {#run-collab-windows}

由于 Windows 系统不支持 `foreman`，您需要在独立的终端中分别运行以下命令：

```powershell
cargo run --package=collab -- serve all
```

如果你已将 `livekit-server` 二进制文件添加到 `PATH` 中，你可以运行：

```powershell
livekit-server --dev
```

否则，

```powershell
.\path\to\livekit-serve.exe --dev
```

你还需要启动云服务器：

```powershell
cd ..\cloud; cargo make dev
```

在新的终端中，运行两个或更多 Zed 实例。

```powershell
node .\script\zed-local -2
```

请注意，这需要 `node.exe` 位于你的 `PATH` 中。

## 运行本地协作服务器

> [!注意]
> 由于我们认证系统最近的更改，Zed 将无法与本地协作服务器进行认证，因此无法使用它。

如果你想运行自己的 Zed 协作服务版本，你可以这样做，但请注意这仍在开发中，并且不支持认证和扩展。

配置通过环境变量完成。默认情况下，系统会从[`.env.toml`](https://github.com/zed-industries/zed/blob/main/crates/collab/.env.toml)读取配置，您可参照该文件进行设置。

Zed默认将DATABASE_URL视为Postgres数据库，但您可以通过以下方式改用Sqlite：编译时添加`--features sqlite`，并在DATABASE_URL中使用`?mode=rwc`指定Sqlite路径。

身份验证需先配置服务器：创建至少包含您GitHub账号的seed.json文件，系统将据此按需创建用户。

```json [settings]
{
  "admins": ["nathansobo"]
}
```

默认情况下，协作服务器在首次创建数据库时会进行初始化。如需添加更多用户，可通过执行`SEED_PATH=./seed.json cargo run -p collab seed`命令显式重新初始化。

然后在运行zed客户端时，必须设置两个环境变量：`ZED_ADMIN_API_TOKEN`（其值需与.env.toml中的`API_TOKEN`保持一致）和`ZED_IMPERSONATE`（其值需对应seed.json中的某个用户）。