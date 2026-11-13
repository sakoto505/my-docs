# 使用指南

本页记录我日常维护这个文档站点的流程，以及在本地排查问题的方法。

## 日常维护流程

1. 修改 `docs/` 目录中的 Markdown 文件。
2. 在本地预览并检查页面及链接。
3. 提交修改并推送到 `main` 分支。
4. 在 Actions 页面查看构建与部署状态。
5. 打开网站检查内容是否更新。

## 常用命令

| 命令 | 用途 |
| --- | --- |
| `python -m mkdocs serve` | 本地预览，默认 <http://127.0.0.1:8000/>，保存后自动刷新 |
| `python -m mkdocs build --strict` | 构建网站，遇到警告即失败 |
| `python -m mkdocs build` | 普通构建，产物输出到 `site/` |
| `git push origin main` | 推送后自动触发部署工作流 |

## 环境准备

建议使用独立虚拟环境，避免污染系统 Python：

```bash
python -m venv .venv
```

激活虚拟环境（按终端类型选一种）：

```bash
# Windows Git Bash
source .venv/Scripts/activate

# Windows PowerShell
.\.venv\Scripts\Activate.ps1

# macOS / Linux
source .venv/bin/activate
```

安装依赖并构建：

```bash
python -m pip install -r requirements.txt
python -m mkdocs build --strict
```

## 新增一个页面

假设要加一页「常见问题」：

1. 新建 `docs/faq.md`，写正常 Markdown 即可；
2. 在 `mkdocs.yml` 的 `nav` 中追加一行（路径相对 `docs/`）：

```yaml
nav:
  - 首页: index.md
  - 关于: about.md
  - 使用指南: guide.md
  - API参考: api.md
  - 常见问题: faq.md
```

3. 本地 `build --strict` 通过后推送。

> 注意：如果新建了页面却没有加进 `nav`，`--strict` 会因为"文件未包含在导航中"而报警告。
> 这正是我坚持用 `--strict` 的原因——它替我记得这类容易遗漏的步骤。

## 常见问题排查

| 现象 | 可能原因 | 处理方式 |
| --- | --- | --- |
| Actions 里 build 失败 | `nav` 中的路径写错，或文件未提交 | 看日志里第一个 `ERROR`，核对文件路径与大小写 |
| 网站能打开但样式丢失 | `site_url` 与仓库名不匹配 | 确认 `site_url` 为 `https://用户名.github.io/仓库名/` |
| 推送后没有触发运行 | 只改了 `report/` 或 `README.md` | 工作流的 `paths` 只监听 `docs/**` 等文件，属预期行为 |
| 页面还是旧内容 | 浏览器或 CDN 缓存 | 强制刷新或使用无痕窗口 |

## 我的使用感受

有了自动部署之后，"更新文档"这件事的心理成本明显降低：不再需要专门留出一段时间去构建、
上传、验证，改完推送就行。真正需要我判断的只剩两件事——**内容写对了吗**、**链接通不通**，
其余交给 CI。
