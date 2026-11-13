# 我的技术文档

> 开源软件开发实践 · 实践项目 2：通过 GitHub Actions 自动部署静态文档到 GitHub Pages

## 项目简介

本项目使用 **Markdown + MkDocs Material** 编写一份四页的技术文档网站，并通过 **GitHub Actions**
实现「推送文档源码 → 自动构建 → 自动部署 → 网页更新」的全自动发布流程。

整个过程中我只需要关心 `docs/` 目录里的 Markdown 内容，构建产物 `site/` 由 CI 在云端生成并直接
上传为 Pages 制品，既不需要手工 `mkdocs gh-deploy`，也不需要维护 `gh-pages` 分支。

## 网站地址

- 最终网站：<https://sakoto505.github.io/my-docs/>

## 报告入口

- [个人实验报告](report/README.md)

## 本地预览

```bash
python -m venv .venv
# Windows PowerShell
.\.venv\Scripts\Activate.ps1
# Windows Git Bash / macOS / Linux
source .venv/bin/activate

python -m pip install -r requirements.txt
python -m mkdocs build --strict
python -m mkdocs serve      # 默认 http://127.0.0.1:8000/
```

## 仓库结构

```text
my-docs/
├── .github/workflows/deploy.yml   # 自动构建并部署到 Pages
├── docs/                          # 网站内容（唯一需要手工编写的部分）
│   ├── index.md
│   ├── about.md
│   ├── guide.md
│   └── api.md
├── report/                        # 实验报告与过程截图
├── mkdocs.yml                     # 站点配置（导航、主题）
├── requirements.txt               # 构建依赖
└── .gitignore                     # 忽略 site/ 与虚拟环境
```

## 许可

本项目仅用于课程学习与作业展示。
