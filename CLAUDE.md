# CLAUDE.md - 项目指导文档

## 项目概述
- **项目名称**：mijiu博客
- **主要功能**：博客
- **技术栈**：django+pymysql
- **项目状态**：已完成

## 项目结构

项目根目录/
├── .idea/              # PyCharm IDE 配置目录（开发工具相关）
├── blog/              # 博客应用（Django app）
├── mijiublog/         # 主应用（Django app）
├── static/            # 静态文件目录
├── templates/         # 模板文件目录
├── zlauth/            # 用户认证应用（Django app）
├── CLAUDE.md          # Claude 指导文档
├── manage.py          # Django 管理脚本
└── my.cnf            # MySQL 配置文件（可能为备份或备用配置）

## 开发环境
### IDE 配置
- 项目使用 **PyCharm** 开发
- `.idea/` 目录包含 IDE 特定配置
- 建议：将 `.idea/` 添加到 `.gitignore`（如果使用 Git）

### 依赖管理
检查是否存在以下文件：
- `requirements.txt` - Python 依赖包列表
- `Pipfile` - 如果使用 pipenv
- `pyproject.toml` - 如果使用现代 Python 工具链

## Django 应用架构
### 应用职责划分
| 应用名称      | 功能描述                       | 最后更新时间 |
| ------------- | ------------------------------ | ------------ |
| **zlauth**    | 用户认证、登录、注册、权限管理 | 2024-04-04   |
| **blog**      | 主要博客内容管理系统           | 2024-04-05   |
| **mijiublog** | 主系统                         | 2026-01-03   |
| **static**    | 静态资源文件（CSS, JS, 图片）  | 2024-04-05   |
| **templates** | Django HTML 模板               | 2026-01-03   |

## 运行与开发指令
```bash
# 1. 环境准备
python -m venv venv           # 创建虚拟环境
source venv/bin/activate     # 激活虚拟环境（Linux/Mac）
# venv\Scripts\activate      # 激活虚拟环境（Windows）

# 2. 安装依赖
pip install django           # 如果没有 requirements.txt
# 或
pip install -r requirements.txt

# 3. 数据库设置
python manage.py makemigrations
python manage.py migrate

# 4. 启动开发服务器
python manage.py runserver

# 5. 创建管理员（首次运行）
python manage.py createsuperuser