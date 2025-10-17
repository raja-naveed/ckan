# Snap4IDTCity Data Portal - CKAN Installation

A customized CKAN (Comprehensive Knowledge Archive Network) data portal for Snap4IDTCity with a modern, user-friendly interface.

## 📋 Overview

This is a traditional source installation of CKAN 2.10.5 with a custom theme extension (`ckanext-custom-theme`) that provides:

- Modern, government-style UI with custom header and footer
- Hero section with prominent search functionality
- Dataset statistics display
- Category browsing with visual cards
- Fully responsive design

## 🚀 Features

- **Custom Theme**: Modern, clean interface with government portal styling
- **Search Functionality**: Prominent search box on homepage
- **Statistics Dashboard**: Real-time display of datasets, resources, organizations, and topics
- **Category Navigation**: Visual category cards for easy browsing
- **Responsive Design**: Works seamlessly on desktop and mobile devices

## 📦 Installation

### Prerequisites

- Python 3.11
- PostgreSQL
- Solr 8.11.2
- Redis

### Setup Instructions

1. **Clone the repository**
   ```bash
   git clone <your-repo-url>
   cd ckan
   ```

2. **Create and activate virtual environment**
   ```bash
   python -m venv venv311
   # On Windows:
   .\venv311\Scripts\activate
   # On Linux/Mac:
   source venv311/bin/activate
   ```

3. **Install dependencies**
   ```bash
   pip install -e .
   pip install -r requirements.txt
   ```

4. **Install custom theme extension**
   ```bash
   cd ckanext-custom-theme
   pip install -e .
   cd ..
   ```

5. **Configure CKAN**
   - Copy `development.ini.example` to `development.ini` (create your own config)
   - Update database connection settings
   - Update Solr URL
   - Update Redis URL
   - Set `ckan.plugins` to include `custom_theme`

6. **Initialize database**
   ```bash
   ckan -c development.ini db init
   ```

7. **Create admin user**
   ```bash
   ckan -c development.ini sysadmin add admin
   ```

8. **Start the services**
   
   **Start Solr:**
   ```bash
   cd C:\solr\solr-8.11.2\bin
   .\solr.cmd start
   ```
   
   **Start Redis:**
   ```bash
   redis-server
   ```
   
   **Start CKAN:**
   ```bash
   cd C:\Users\Raja\Desktop\Projects\NewCkan\ckan
   .\venv311\Scripts\activate
   ckan -c development.ini run
   ```

9. **Access the portal**
   - Open your browser and navigate to `http://localhost:5000`

## 🎨 Custom Theme

The custom theme is located in `ckanext-custom-theme/` and provides:

### Features
- **Custom Header**: Branding with government emblem, navigation menu, search box, and login link
- **Custom Footer**: Multi-column footer with links, social media icons, and copyright information
- **Custom Homepage**: Hero section, statistics, and category cards
- **Responsive Design**: Mobile-friendly layout

### Structure
```
ckanext-custom-theme/
├── ckanext/
│   └── custom_theme/
│       ├── plugin.py          # Plugin configuration and helper functions
│       ├── templates/          # Jinja2 templates
│       │   ├── base.html      # Base template with global styles
│       │   ├── header.html    # Custom header
│       │   ├── footer.html    # Custom footer
│       │   └── home/
│       │       └── index.html # Custom homepage
│       └── public/            # Static assets (CSS, JS, images)
└── setup.py
```

### Customization

To customize the theme, edit the files in `ckanext-custom-theme/ckanext/custom_theme/templates/`:

- **Colors**: Modify the color variables in `base.html`
- **Header/Footer**: Edit `header.html` and `footer.html`
- **Homepage**: Modify `home/index.html`

## 🔧 Configuration

### Enabled Plugins

The following plugins are enabled in `development.ini`:

```ini
ckan.plugins = stats text_view image_view custom_theme
```

### Site Configuration

```ini
ckan.site_title = Snap4IDTCity Data Portal
ckan.site_description = Providing access to government data
```

## 📝 Development

### File Structure

```
ckan/
├── ckan/                      # CKAN core application
├── ckanext-custom-theme/      # Custom theme extension
├── venv311/                   # Python virtual environment (not in git)
├── development.ini            # Configuration file (not in git)
├── requirements.txt           # Python dependencies
├── setup.py                   # CKAN setup file
└── README.md                  # This file
```

### Making Changes

1. **Template Changes**: Edit files in `ckanext-custom-theme/ckanext/custom_theme/templates/`
2. **Plugin Changes**: Edit `ckanext-custom-theme/ckanext/custom_theme/plugin.py`
3. **Restart CKAN**: After making changes, restart the CKAN server

### Known Issues

1. **Template Path Issue on Windows**: The custom theme uses direct template extension instead of `{% ckan_extends %}` to avoid Windows path issues with backslashes.

2. **Solr Connection**: Ensure Solr is running before starting CKAN, otherwise the homepage will show errors.

3. **Python 3.11 Compatibility**: Some CKAN functions needed patches for Python 3.11 compatibility (inspect.getargspec → inspect.getfullargspec).

## 🐛 Troubleshooting

### Internal Server Error on Homepage

**Problem**: Homepage shows "Internal server error"

**Solution**: 
1. Check if Solr is running on port 8983
2. Check if Redis is running on port 6379
3. Check CKAN logs for detailed error messages

### Template Not Found Error

**Problem**: `jinja2.exceptions.TemplateNotFound`

**Solution**: The custom theme now uses direct template extension to avoid Windows path issues. Ensure you're using the latest version of `home/index.html`.

### Module Import Errors

**Problem**: `ModuleNotFoundError` when starting CKAN

**Solution**: 
```bash
pip install pyutilib flask sqlalchemy jinja2 alembic babel beaker
pip install psycopg2-binary lxml-html-clean
```

## 📄 License

This project is based on CKAN, which is licensed under the AGPL v3.0 license.

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📧 Contact

For questions or support, please contact: info@dataportal.gov

## 🙏 Acknowledgments

- [CKAN](https://ckan.org/) - The open source data portal platform
- [Font Awesome](https://fontawesome.com/) - Icons used in the interface
- Snap4IDTCity team

---

**Version**: 2.10.5  
**Last Updated**: October 2025

