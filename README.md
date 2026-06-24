# 🛒 RK Store — Multi-Tier E-Commerce Web Application on Azure

> **George Brown College | Cloud Computing Capstone Project**
> A production-grade, enterprise-level e-commerce platform built entirely on Microsoft Azure, featuring real-time inventory management, automated email notifications, autoscaling, and comprehensive monitoring.

![Azure](https://img.shields.io/badge/Azure-App%20Service-0078D4?style=for-the-badge&logo=microsoft-azure)
![Django](https://img.shields.io/badge/Django-4.2-092E20?style=for-the-badge&logo=django)
![Python](https://img.shields.io/badge/Python-3.12-3776AB?style=for-the-badge&logo=python)
![Bootstrap](https://img.shields.io/badge/Bootstrap-5.3-7952B3?style=for-the-badge&logo=bootstrap)

---

## 🎬 Demo Video

[![RK Store Demo Video](https://img.shields.io/badge/▶%20Watch%20Demo-YouTube-FF0000?style=for-the-badge&logo=youtube)](https://youtu.be/rYZhnTK6s0o)

> Click the badge above to watch the full project demo on YouTube.

---

## 🌐 Live Demo

| Resource | URL |
|---|---|
| **🎬 Demo Video** | [Watch on YouTube](https://youtu.be/rYZhnTK6s0o) |
| **RK Store (via Front Door)** | `https://rkstore-endpoint.azurefd.net` |
| **RK Store (App Service)** | `https://app-rkstore-dev-afbyefhngwhsgue0.canadacentral-01.azurewebsites.net` |
| **GitHub Repository** | [Multi-Tier-E-Commerce-Web-Application-on-Azure](https://github.com/shinwari27/Multi-Tier-E-Commerce-Web-Application-on-Azure) |
| **Admin Panel** | `/admin/` |
| **API — Products** | `/api/products/` |
| **API — Orders** | `/api/orders/` |
| **Health Check** | `/health/` |

---

## 🏗️ Architecture

```
                        ┌─────────────────────────────┐
                        │      Azure Front Door        │
                        │  (CDN + WAF + Global LB)     │
                        └──────────────┬──────────────┘
                                       │
                        ┌──────────────▼──────────────┐
                        │      Azure App Service       │
                        │   Python 3.12 + Django 4.2   │
                        │   Gunicorn WSGI Server       │
                        │   Auto-Scale: 1-3 Instances  │
                        │   CPU > 70% → Scale Out      │
                        │   CPU < 30% → Scale In       │
                        └──────┬───────────┬───────────┘
                               │           │
               ┌───────────────▼──┐   ┌────▼──────────────────┐
               │  Azure SQL DB    │   │   Azure Key Vault      │
               │  Products Table  │   │   All Secrets Stored   │
               │  Orders Table    │   │   Managed Identity     │
               │  Customers Table │   │   Zero Hardcoded Keys  │
               └──────────────────┘   └───────────────────────┘
                                               │
               ┌───────────────────────────────▼───────────────┐
               │           Azure Communication Services         │
               │     Email Confirmation + SMS Notifications     │
               └───────────────────────────────────────────────┘
                                               │
               ┌───────────────────────────────▼───────────────┐
               │            Application Insights                │
               │     Real-time Monitoring + Telemetry           │
               └───────────────────────────────────────────────┘
```

---

## ☁️ Azure Services Used

| Service | Purpose |
|---|---|
| **Azure App Service (S1)** | Hosts Django REST API + Frontend |
| **Azure SQL Database** | Stores products, orders, customers |
| **Azure Key Vault** | Secure secret storage (no passwords in code) |
| **Azure Front Door** | Global CDN, WAF, load balancing |
| **Azure Communication Services** | Order confirmation emails + SMS |
| **Application Insights** | Real-time monitoring and telemetry |
| **Azure Monitor** | Autoscale rules and alerts |
| **Azure Load Testing** | Performance and stress testing |
| **GitHub Actions** | CI/CD pipeline — auto-deploy on push |

---

## 🛠️ Tech Stack

### Backend
- **Python 3.12** + **Django 4.2**
- **Django REST Framework** — RESTful API
- **mssql-django** — Azure SQL Database connector
- **ODBC Driver 18** — SQL Server connectivity
- **Gunicorn** — Production WSGI server
- **WhiteNoise** — Static file serving

### Frontend
- **HTML5** + **CSS3** + **JavaScript (ES6+)**
- **Bootstrap 5.3** — Responsive UI framework
- **Bootstrap Icons 1.11** — Icon library
- **Google Fonts (Inter)** — Typography

### Azure SDK
- **azure-keyvault-secrets** — Key Vault integration
- **azure-identity** — Managed Identity authentication
- **azure-communication-email** — Email notifications
- **azure-communication-sms** — SMS notifications
- **opencensus-ext-azure** — Application Insights telemetry

---

## 📦 Features

### 🛍️ Store Features
- **12 iPhone models** — New and Certified Pre-Owned
- **Real-time stock tracking** — Live inventory from Azure SQL
- **Product catalog** — Search + filter (New, Used, Offers, Limited)
- **Product detail modal** — Full specs with animated phone SVG
- **Special offers** — Discounted products with savings display
- **Limited stock alerts** — Urgency indicators with progress bars

### 🛒 Order Management
- **Order form** — Real-time stock validation
- **Instant order confirmation** — RK-XXXXXXX format order numbers
- **Email notification** — Styled HTML email via Azure ACS
- **SMS notification** — Text message confirmation via ACS
- **Stock decrement** — Automatic inventory update on order

### 🔐 Security
- **Azure Key Vault** — All secrets stored securely
- **Managed Identity** — Zero credential code
- **HTTPS enforced** — Via Azure Front Door + App Service
- **HSTS headers** — Browser HTTPS enforcement
- **CSRF protection** — Django middleware
- **SQL firewall** — IP-based access control

### 📊 Monitoring & Scaling
- **Application Insights** — Request tracking, error monitoring
- **Autoscaling** — CPU-based rules (70% scale out, 30% scale in)
- **Min: 1 instance** → **Max: 3 instances**
- **Load tested** — 0 errors at 5 concurrent users, 31 req/sec

---

## 🗄️ Database Schema

```sql
products    → id, name, storage, color, condition, price,
              original_price, stock, warranty, battery_health,
              description, is_featured, is_latest,
              is_special_offer, is_limited, is_active

customers   → id, name, email, phone, address, created_at

orders      → id, order_number, customer_id, status,
              total_amount, email_sent, sms_sent,
              created_at, updated_at

order_items → id, order_id, product_id, quantity,
              unit_price, created_at
```

---

## 🚀 API Endpoints

```
GET  /api/products/          → List all active products
GET  /api/products/<id>/     → Product detail
POST /api/orders/            → Place an order (validates stock)
GET  /api/orders/<id>/       → Order detail
GET  /health/                → Health check (App Service probe)
```

---

## 📁 Project Structure

```
rk-store-azure/
├── manage.py
├── requirements.txt
├── startup.sh               ← Azure App Service startup command
├── .env.example             ← Environment variables template
├── rk_store/
│   ├── settings.py          ← Django settings + Key Vault loader
│   ├── urls.py              ← URL routing
│   └── wsgi.py              ← Gunicorn entry point
├── store/
│   ├── models.py            ← Product, Customer, Order, OrderItem
│   ├── views.py             ← API views with stock validation
│   ├── serializers.py       ← DRF serializers
│   ├── urls.py              ← API URL patterns
│   ├── admin.py             ← Django admin configuration
│   ├── notifications.py     ← ACS email + SMS
│   └── management/
│       └── commands/
│           └── seed_products.py  ← Seed 12 iPhones to DB
└── templates/
    └── index.html           ← Full frontend SPA
```

---

## ⚙️ Local Development Setup

### Prerequisites
- Python 3.12+
- ODBC Driver 18 for SQL Server
- Azure CLI (`az login`)
- Active Azure subscription

### Steps

**1. Clone the repository**
```bash
git clone https://github.com/shinwari27/rk-store-azure.git
cd rk-store-azure
```

**2. Create virtual environment**
```bash
python -m venv venv
venv\Scripts\activate        # Windows
source venv/bin/activate     # Linux/Mac
```

**3. Install dependencies**
```bash
pip install -r requirements.txt
```

**4. Configure environment**
```bash
cp .env.example .env
# Edit .env with your Azure resource details
```

**5. Run migrations**
```bash
python manage.py makemigrations
python manage.py migrate
```

**6. Seed product data**
```bash
python manage.py seed_products
```

**7. Run development server**
```bash
python manage.py runserver
```

---

## 🔑 Key Vault Secrets Required

| Secret Name | Description |
|---|---|
| `django-secret-key` | Django SECRET_KEY |
| `db-server` | Azure SQL server FQDN |
| `db-name` | Database name |
| `db-username` | SQL admin username |
| `db-password` | SQL admin password |
| `acs-email-connection-str` | ACS email connection string |
| `acs-sms-connection-str` | ACS SMS connection string |
| `acs-sender-email` | Verified sender email |
| `acs-sender-phone` | Verified sender phone |
| `appinsights-connection-str` | Application Insights connection |

---

## 📊 Load Test Results

| Test | Users | Duration | Requests | Error Rate | Response Time |
|---|---|---|---|---|---|
| Test 1 | 50 | 2m 8s | 16,261 | 100% (app crash) | 241ms |
| Test 2 | 10 | 2m 16s | 3,002 | 100% (app crash) | 174ms |
| **Test 3** | **5** | **5m 3s** | **~9,000** | **0% ✅** | **118ms** |

Test 3 passed with zero errors after fixing the Application Insights middleware conflict.

---

## 🎓 Academic Information

| Detail | Value |
|---|---|
| **Institution** | George Brown College |
| **Program** | Cloud Computing — Postgraduate Certificate |
| **Course** | Cloud Computing Capstone |
| **Student** | Hikmatullah Shinwari |
| **GitHub** | [@shinwari27](https://github.com/shinwari27) |
| **Repository** | [Multi-Tier-E-Commerce-Web-Application-on-Azure](https://github.com/shinwari27/Multi-Tier-E-Commerce-Web-Application-on-Azure) |
| **Demo Video** | [Watch on YouTube](https://youtu.be/rYZhnTK6s0o) |

---

## 📄 License

This project was built for academic purposes at George Brown College.

---

*Built with ❤️ on Microsoft Azure — Canada Central*
