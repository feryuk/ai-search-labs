# Deployment Instructions for Siteground

## Pre-Deployment Steps (Already Completed)

1. ✅ Production assets built: `npm run build`
2. ✅ Laravel optimized: Config, routes, and views cached
3. ✅ Production .env template created

## Siteground Deployment Steps

### 1. Database Setup (Siteground Control Panel)
1. Log into your Siteground control panel
2. Go to Site Tools > MySQL > Databases
3. Create a new database and note down:
   - Database name
   - Database username  
   - Database password
   - Host (usually localhost or 127.0.0.1)

### 2. Configure .env File
1. Copy `.env.production` to `.env`
2. Update these values with your Siteground details:
   ```
   APP_URL=https://yourdomain.com
   DB_DATABASE=your_database_name
   DB_USERNAME=your_database_user
   DB_PASSWORD=your_database_password
   MAIL_FROM_ADDRESS="hello@yourdomain.com"
   ```

### 3. Files to Upload via FTP

Since your project root is linked to public_html, upload ALL these files/folders:

**Essential Folders:**
- `/app` - Application logic
- `/bootstrap` - Laravel bootstrap files
- `/config` - Configuration files
- `/database` - Migrations and seeders
- `/public` - Public assets (IMPORTANT: includes /build folder)
- `/resources` - Views and raw assets
- `/routes` - Application routes
- `/storage` - Storage (set permissions to 755)
- `/vendor` - PHP dependencies (if composer not available on server)

**Essential Files:**
- `.env` (the configured production version)
- `.htaccess` (if not in public folder)
- `artisan` - Laravel CLI
- `composer.json` and `composer.lock`
- All files in root directory

**DO NOT Upload:**
- `/node_modules` - Not needed in production
- `.git` - Version control files
- `.env.example` or `.env.production` - Templates only
- `CLAUDE.md` - Development notes
- Any cache files in `/storage/framework/`

### 4. Siteground-Specific Configuration

#### A. Update public/.htaccess
Make sure your `public/.htaccess` file has these rules:
```apache
<IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteBase /
    RewriteRule ^index\.php$ - [L]
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule . /index.php [L]
</IfModule>
```

#### B. If your domain points to public_html root:
Create an `.htaccess` in the root with:
```apache
<IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteRule ^(.*)$ public/$1 [L]
</IfModule>
```

### 5. Post-Upload Steps (via SSH or Siteground Terminal)

If you have SSH access:
```bash
# Navigate to your site directory
cd ~/public_html

# Install composer dependencies (if vendor not uploaded)
composer install --optimize-autoloader --no-dev

# Run migrations
php artisan migrate --force

# Set proper permissions
chmod -R 755 storage
chmod -R 755 bootstrap/cache

# Clear and rebuild caches
php artisan cache:clear
php artisan config:cache
php artisan route:cache
php artisan view:cache
```

If NO SSH access, use Siteground's Site Tools > Devs > PHP Terminal.

### 6. File Permissions
Set via FTP or File Manager:
- `storage/` - 755 (recursive)
- `bootstrap/cache/` - 755 (recursive)
- `.env` - 644

### 7. PHP Version
Ensure PHP 8.2+ is selected in Siteground:
- Site Tools > Devs > PHP Manager
- Select PHP 8.2 or higher

### 8. SSL Configuration
If using HTTPS (recommended):
- Site Tools > Security > SSL Manager
- Install Let's Encrypt SSL
- Force HTTPS redirect

## Troubleshooting

### White Screen/500 Error
1. Check `.env` file exists and is configured
2. Check `storage/logs/laravel.log` for errors
3. Verify database credentials
4. Ensure APP_KEY is set in .env

### Assets Not Loading
1. Check `public/build` folder was uploaded
2. Verify APP_URL in .env matches your domain
3. Clear browser cache

### Database Connection Error
1. Verify database credentials in .env
2. Check if database exists
3. Ensure DB_HOST is correct (usually localhost)

### Permission Denied Errors
```bash
chmod -R 755 storage
chmod -R 755 bootstrap/cache
```

## Quick Checklist
- [ ] Database created and configured
- [ ] .env file configured with production values
- [ ] All files uploaded except node_modules
- [ ] public/build folder included
- [ ] File permissions set (storage, bootstrap/cache)
- [ ] PHP 8.2+ selected
- [ ] SSL configured
- [ ] Migrations run
- [ ] Caches cleared and rebuilt

## Important Notes
- The compiled assets are in `public/build/` - this MUST be uploaded
- Never upload `.env.example` as `.env` without configuring it
- Keep your local `.env` for development, use production values on server
- If making changes, rebuild assets locally with `npm run build` before uploading