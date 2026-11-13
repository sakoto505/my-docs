# 关于本项目

## 项目目标

实现从 Markdown 文档到公开网站的自动化发布：我在本地写 Markdown，推送到 `main` 分支之后，
构建和部署由 GitHub Actions 自动完成，读者通过一个固定的网址访问最新版本。

## 技术组成

| 工具 | 作用 |
| --- | --- |
| Markdown | 编写文档内容 |
| MkDocs Material | 将文档构建成静态网站 |
| GitHub Actions | 自动执行安装、构建和部署 |
| GitHub Pages | 托管并提供网站访问地址 |

## 三个目录的分工

初次接触这套工具时，最容易混淆的是 `docs/`、`site/` 和 `.github/workflows/` 三者的关系。
我用一句话概括：

> `docs/` 是我写的，`site/` 是机器生成的，`workflows/` 是告诉机器怎么做的。

| 路径 | 是否手写 | 是否进仓库 | 说明 |
| --- | --- | --- | --- |
| `docs/` | 是 | 是 | 文档源文件，MkDocs 的输入 |
| `mkdocs.yml` | 是 | 是 | 站点配置，`nav` 里的路径相对于 `docs/` |
| `site/` | 否 | 否（已 ignore） | 构建产物，由 CI 上传为 Pages 制品 |
| `.github/workflows/deploy.yml` | 是 | 是 | 定义构建与部署两个任务 |

## 构建流程

```text
docs/*.md + mkdocs.yml
        │
        │  mkdocs build --strict
        ▼
      site/  ──upload-pages-artifact──►  Pages 制品
                                              │
                                              │  deploy-pages
                                              ▼
                                    https://sakoto505.github.io/my-docs/
```

## 为什么导航里不写 `docs/index.md`

`mkdocs.yml` 的 `nav` 是**相对于 `docs_dir`** 解析的，而 `docs_dir` 默认就是 `docs/`。
如果写成 `docs/index.md`，MkDocs 会去找 `docs/docs/index.md`，构建时直接报文件不存在。
这一点我在第一次配置时就踩过，属于"看起来很像、实际不生效"的典型问题。

## 我理解的关键点

- 网站不是"另一份内容"，而是同一份 Markdown 的**渲染结果**，所以只要源文件对，页面就不会不一致。
- Pages 提供的是**静态托管**，没有后端逻辑；所有"动态"能力都必须在构建期完成。
- 自动化流程的价值在于**可重复**：同样的输入永远得到同样的输出，出错时也能从日志里定位。
