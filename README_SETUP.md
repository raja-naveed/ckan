# CKAN Setup Guide - Snap4IDTCity Data Portal

## 📋 Overview

This is a complete CKAN 2.10.5 installation on Windows with a custom theme called **"Snap4IDTCity Data Portal"**.

## 🔧 System Requirements

- **Python 3.11** (installed in `venv311/`)
- **PostgreSQL 15** (Database)
- **Apache Solr 8.11.2** (Search Engine)
- **Redis** (Background Jobs)
- **Java 17** (Required for Solr)

---

## 🚀 Quick Start Guide

### **Step 1: Start Required Services**

Before starting CKAN, you need to start these three services:

#### **1. Start PostgreSQL**
PostgreSQL should start automatically as a Windows service. To check:
```powershell
Get-Service -Name "postgresql*"
```

If not running, start it:
```powershell
Start-Service -Name "postgresql-x64-15"
```

#### **2. Start Solr**
```powershell
cd C:\solr\solr-8.11.2\bin
.\solr.cmd start
```

Expected output: `Started Solr server on port 8983. Happy searching!`

#### **3. Start Redis**
```powershell
Start-Process -FilePath "C:\redis\redis-server.exe" -WindowStyle Minimized
```

---

### **Step 2: Start CKAN**

1. **Navigate to CKAN directory:**
```powershell
cd C:\Users\Raja\Desktop\Projects\NewCkan\ckan
```

2. **Activate virtual environment and start CKAN:**
```powershell
.\venv311\Scripts\activate
ckan -c development.ini run
```

3. **Access CKAN:**
   - Open your browser and go to: **http://localhost:5000**

---

## 🔑 Login Credentials

- **URL:** http://localhost:5000
- **Username:** `admin`
- **Password:** [The password you set during setup]

---

## 🛠️ Useful Commands

### **Check Service Status**

**Check if Solr is running:**
```powershell
Invoke-WebRequest -Uri "http://localhost:8983/solr/admin/cores?action=STATUS" -UseBasicParsing
```

**Check if Redis is running:**
```powershell
Get-Process | Where-Object {$_.ProcessName -like "*redis*"}
```

**Check if CKAN is running:**
```powershell
Get-Process | Where-Object {$_.ProcessName -like "*python*"}
```

---

### **Stop Services**

**Stop Solr:**
```powershell
cd C:\solr\solr-8.11.2\bin
.\solr.cmd stop -all
```

**Stop Redis:**
```powershell
Stop-Process -Name "redis-server" -Force
```

**Stop CKAN:**
Press `Ctrl+C` in the terminal where CKAN is running, or:
```powershell
taskkill /F /IM python.exe
```

---

### **Restart Services**

**Restart Solr:**
```powershell
cd C:\solr\solr-8.11.2\bin
.\solr.cmd restart
```

**Restart CKAN:**
```powershell
taskkill /F /IM python.exe
.\venv311\Scripts\activate
ckan -c development.ini run
```

---

## 📁 Directory Structure

```
C:\Users\Raja\Desktop\Projects\NewCkan\ckan\
├── venv311/                          # Python 3.11 virtual environment
├── ckan/                             # CKAN core source code
├── ckanext-custom-theme/             # Custom theme extension
│   └── ckanext/custom_theme/
│       ├── templates/                # Custom templates
│       ├── public/                   # Static files (CSS, images)
│       └── plugin.py                 # Theme plugin
├── development.ini                   # CKAN configuration file
└── README_SETUP.md                   # This file
```

---

## 🎨 Custom Theme Features

The **Snap4IDTCity Data Portal** custom theme includes:

- ✅ **Custom Header** with government branding
- ✅ **Hero Section** with search functionality
- ✅ **Category Grid** for browsing by topic (8 categories)
- ✅ **Statistics Dashboard** showing dataset counts
- ✅ **Responsive Design** with modern styling
- ✅ **Custom Footer** with additional links

---

## 🔧 Configuration

### **Enabled Plugins** (in `development.ini`):
```ini
ckan.plugins = stats text_view image_view recline_view custom_theme
```

### **Database Connection:**
```ini
sqlalchemy.url = postgresql://ckan_default:pass@localhost/ckan_default
```

### **Solr Connection:**
```ini
solr_url = http://127.0.0.1:8983/solr/ckan
```

### **Site Settings:**
```ini
ckan.site_url = http://localhost:5000
ckan.site_title = Snap4IDTCity Data Portal
ckan.site_id = default
```

---

## 🐛 Troubleshooting

### **Issue: "Internal Server Error" on homepage**

**Possible causes:**
1. Solr is not running
2. PostgreSQL is not running
3. Redis is not running

**Solution:**
Start all services as described in Step 1 above.

---

### **Issue: Solr connection error**

**Error:** `ConnectionRefusedError: [WinError 10061]`

**Solution:**
```powershell
# Check if Solr is running
netstat -an | findstr :8983

# If not running, start Solr
cd C:\solr\solr-8.11.2\bin
.\solr.cmd start
```

---

### **Issue: Template errors**

**Error:** `jinja2.exceptions.TemplateNotFound`

**Solution:**
Make sure the custom theme is properly installed:
```powershell
cd ckanext-custom-theme
pip install -e . --force-reinstall
```

---

### **Issue: Plugin not found**

**Error:** `PluginNotFoundException: custom_theme`

**Solution:**
1. Make sure the theme is installed:
```powershell
cd ckanext-custom-theme
pip install -e .
```

2. Check if the plugin is listed:
```powershell
python -c "import pkg_resources; print([ep.name for ep in pkg_resources.iter_entry_points('ckan.plugins')])"
```

---

## 📊 CKAN Administration

### **Create a new admin user:**
```powershell
.\venv311\Scripts\activate
ckan -c development.ini sysadmin add USERNAME email=user@example.com name=USERNAME
```

### **Initialize/upgrade database:**
```powershell
ckan -c development.ini db init
```

### **Rebuild Solr index:**
```powershell
ckan -c development.ini search-index rebuild
```

---

## 🔄 Development Workflow

### **Make changes to the theme:**

1. Edit template files in `ckanext-custom-theme/ckanext/custom_theme/templates/`
2. Edit CSS files in `ckanext-custom-theme/ckanext/custom_theme/public/css/`
3. Restart CKAN to see changes:
```powershell
# Press Ctrl+C to stop CKAN
ckan -c development.ini run
```

### **Install additional extensions:**

1. Navigate to the extension directory
2. Install it in development mode:
```powershell
pip install -e .
```
3. Add the extension to `development.ini` plugins list
4. Restart CKAN

---

## 📦 Service Locations

| Service     | Location                                    | Port |
|-------------|---------------------------------------------|------|
| CKAN        | C:\Users\Raja\Desktop\Projects\NewCkan\ckan | 5000 |
| Solr        | C:\solr\solr-8.11.2                         | 8983 |
| Redis       | C:\redis                                    | 6379 |
| PostgreSQL  | C:\Program Files\PostgreSQL\15              | 5432 |

---

## 🌐 Useful URLs

- **CKAN Homepage:** http://localhost:5000
- **CKAN Admin:** http://localhost:5000/ckan-admin
- **CKAN API:** http://localhost:5000/api/3/action/package_list
- **Solr Admin:** http://localhost:8983/solr/#/ckan

---

## 📝 Notes

- **Python Version:** This installation requires Python 3.11 (CKAN 2.10.5 does not support Python 3.12+)
- **Windows Compatibility:** Some packages required workarounds:
  - `psycopg2-binary` instead of `psycopg2`
  - Custom `magic.py` shim for file type detection
  - UTF-8 encoding fix in `ckan/config/declaration/load.py`

---

## 🎯 Next Steps

1. **Add Data:** Create organizations, datasets, and resources
2. **Customize Theme:** Modify templates and styles to match your needs
3. **Install Extensions:** Add more functionality (e.g., scheming, harvest, etc.)
4. **Configure Email:** Set up email notifications in `development.ini`
5. **Set up Production:** For production deployment, use Apache/NGINX with uWSGI

---

## 📞 Support

For CKAN documentation, visit: https://docs.ckan.org/

For issues specific to this installation, check the troubleshooting section above.

---

**Last Updated:** October 17, 2025
**CKAN Version:** 2.10.5
**Theme:** Snap4IDTCity Data Portal (custom_theme)

