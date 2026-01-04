# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview
- **Name**: mijiu博客 (MiJiu Blog)
- **Framework**: Django 5.0.3
- **Database**: MySQL (via PyMySQL)
- **Language**: Chinese (zh-hans)
- **Timezone**: Asia/Shanghai

## Critical Configuration Issues

**IMPORTANT**: The project has naming inconsistencies that must be understood before making changes:

1. **Settings Module Mismatch**:
   - `manage.py` references `zhiliaoblog.settings` (line 9)
   - But the actual directory is named `mijiublog/`
   - URL configs also reference `zhiliaoblog.urls` and `zhiliaoblog.wsgi`
   - **Action**: When modifying settings imports, use `zhiliaoblog` (not `mijiublog`)

2. **Database Configuration**:
   - Database credentials are stored in `my.cnf` (NOT in version control safe location)
   - Database name: `mijiublog`
   - User: `root`
   - Contains plain-text password - should be moved to environment variables

## Project Structure

```
mijiublog/                    # Project root
├── mijiublog/               # Main Django project package (referenced as 'zhiliaoblog')
│   ├── settings.py          # Settings module: 'zhiliaoblog.settings'
│   ├── urls.py              # Root URL config
│   └── wsgi.py
├── blog/                    # Blog functionality app
├── zlauth/                  # Authentication app
├── static/                  # Static files (CSS, JS, images)
├── templates/               # Global templates
├── my.cnf                   # MySQL credentials file
└── manage.py
```

## Django Apps Architecture

### blog (Blog Content Management)
- **URL Namespace**: `blog:`
- **URL Prefix**: `/` (root)
- **Models**:
  - `BlogCategory`: Blog categories
  - `Blog`: Blog posts with title, content, pub_time, category (FK), author (FK)
  - `BlogComment`: Comments with content, pub_time, blog (FK), author (FK)
- **Key Views**:
  - `index`: Homepage/blog list
  - `blog_detail`: Individual blog post (`/blog/detail/<int:blog_id>`)
  - `pub_blog`: Publish new blog
  - `pub_comment`: Publish comment
  - `search`: Search blogs
- **Ordering**: Both Blog and BlogComment ordered by `-pub_time` (newest first)

### zlauth (User Authentication)
- **URL Namespace**: `zlauth:`
- **URL Prefix**: `/auth/`
- **Models**:
  - `CaptchaModel`: Email verification codes (unique email, 4-char captcha, create_time)
- **Key Views**:
  - `zllogin`: Login (`/auth/login`)
  - `zllogout`: Logout (`/auth/logout`)
  - `register`: Registration (`/auth/register`)
  - `send_email_captcha`: Send email captcha (`/auth/captcha`)
- **Email Backend**: Configured for QQ SMTP (smtp.qq.com:587 with TLS)

## Development Commands

```bash
# Navigate to project directory
cd mijiublog

# Database migrations
python manage.py makemigrations
python manage.py migrate

# Run development server
python manage.py runserver

# Run on specific host (for network access)
python manage.py runserver 0.0.0.0:8000

# Create superuser
python manage.py createsuperuser

# Access admin panel
# Navigate to http://localhost:8000/admin/

# Django shell
python manage.py shell

# Collect static files (production)
python manage.py collectstatic
```

## Key Settings

- **Static Files**:
  - `STATIC_URL = 'static/'`
  - `STATICFILES_DIRS = [BASE_DIR / "static"]`
  - Template builtins include `django.templatetags.static` for `{% static %}` tag

- **Templates**:
  - Located in project-level `templates/` directory
  - Template context includes standard Django processors

- **Authentication**:
  - Uses Django's default User model (via `get_user_model()`)
  - `LOGIN_URL = '/auth/login'`
  - Standard password validators enabled

- **Admin Interface**:
  - Available at `/admin/`
  - Both apps register their models in admin

## Development Notes

1. **Database Access**: MySQL connection configured via `my.cnf` file (not environment variables)

2. **Email Configuration**: Uses QQ SMTP for sending captcha emails - credentials hardcoded in settings.py

3. **Security Concerns** (for production):
   - `DEBUG = True` is enabled
   - `SECRET_KEY` is exposed in settings.py
   - Database credentials in `my.cnf` without encryption
   - Email password hardcoded
   - Consider using environment variables or django-environ

4. **Related Names**: BlogComment uses `related_name='comments'` for reverse relation from Blog

5. **Foreign Key Cascades**: All FKs use `on_delete=models.CASCADE`

## URL Structure

```
/                          → blog.views.index
/blog/detail/<id>          → blog.views.blog_detail
/blog/pub                  → blog.views.pub_blog
/blog/comment/pub          → blog.views.pub_comment
/search                    → blog.views.search
/auth/login                → zlauth.views.zllogin
/auth/logout               → zlauth.views.zllogout
/auth/register             → zlauth.views.register
/auth/captcha              → zlauth.views.send_email_captcha
/admin/                    → Django admin
```
