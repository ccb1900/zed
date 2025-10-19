# SQL

SQL 文件由 [SQL 扩展](https://github.com/zed-extensions/sql) 处理。

- Tree-sitter 解析器：[nervenes/tree-sitter-sql](https://github.com/nervenes/tree-sitter-sql)

### 格式化

Zed 支持使用外部工具（如 [`sql-formatter`](https://github.com/sql-formatter-org/sql-formatter)）自动格式化 SQL 代码。

1. 安装 `sql-formatter`：

```sh
npm install -g sql-formatter
```

2. 确保 `sql-formatter` 在您的路径中可用，并检查版本：

```sh
which sql-formatter
sql-formatter --version
```

3. 配置 Zed 以使用 `sql-formatter` 自动格式化 SQL：

```json [settings]
  "languages": {
    "SQL": {
      "formatter": {
        "external": {
          "command": "sql-formatter",
          "arguments": ["--language", "mysql"]
        }
      }
    }
  },
```

将上述的 `mysql` 替换为您偏好的 [SQL 方言]（例如 `duckdb`、`hive`、`mariadb`、`postgresql`、`redshift`、`snowflake`、`sqlite`、`spark` 等）。

你可以将此添加到 Zed 项目设置（`.zed/settings.json`）或通过你的 Zed 用户设置（`~/.config/zed/settings.json`）进行配置。

### 高级格式化

Sql-formatter 还支持通过 [sql-formatter 配置选项](https://github.com/sql-formatter-org/sql-formatter#configuration-options)实现更精细的控制。要使用此功能，请在项目中创建 `.sql-formatter.json` 文件：

```json [settings]
{
  "language": "postgresql",
  "tabWidth": 2,
  "keywordCase": "upper",
  "linesBetweenQueries": 2
}
```

当使用 `.sql-formatter.json` 文件时，由于无需在代码中指定语言，你可以采用更简化的 Zed 设置：

```json [settings]
  "languages": {
    "SQL": {
      "formatter": {
        "external": {
          "command": "sql-formatter"
        }
      }
    }
  },
```