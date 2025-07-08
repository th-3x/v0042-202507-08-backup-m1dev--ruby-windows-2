# Rails 8 Installation Verification Script for Windows

## System Status Check ✅

Your Windows system has a complete Ruby on Rails 8 development environment:

### Core Components Installed:
- **Ruby**: 3.4.4 (2025-05-14 revision a38531fd3f) +PRISM [x64-mingw-ucrt]
- **Rails**: 8.0.2
- **Node.js**: v22.15.0
- **Yarn**: 1.22.22
- **Bundler**: 2.6.9
- **Git**: 2.49.0.windows.1
- **SQLite**: 3.50.0

## Verification Commands

Run these commands to verify your installation:

```cmd
# Check Ruby version
ruby --version
# Expected: ruby 3.4.4 (2025-05-14 revision a38531fd3f) +PRISM [x64-mingw-ucrt]

# Check Rails version
rails --version
# Expected: Rails 8.0.2

# Check Node.js version
node --version
# Expected: v22.x or later

# Check Yarn version
yarn --version
# Expected: 1.22.x

# Check Bundler version
bundle --version
# Expected: Bundler version 2.6.x

# Check Git version
git --version
# Expected: git version 2.x

# Check SQLite version
sqlite3 --version
# Expected: 3.x
```

## Rails 8 Features Test

### 1. Create a New Rails 8 Application
```cmd
rails new my_rails8_app --database=sqlite3
cd my_rails8_app
```

### 2. Install Dependencies
```cmd
bundle install
```

### 3. Generate a Simple Controller
```cmd
rails generate controller Welcome index
```

### 4. Update Routes
Edit `config/routes.rb`:
```ruby
Rails.application.routes.draw do
  root 'welcome#index'
  get 'welcome/index'
end
```

### 5. Start Development Server
```cmd
rails server
```
Open browser to: http://localhost:3000

## Rails 8 New Features Demonstration

### Propshaft Asset Pipeline
Rails 8 uses Propshaft instead of Sprockets:
- Check `app/assets/config/manifest.js`
- CSS and JavaScript files are served directly
- No asset compilation needed in development

### Importmap for JavaScript
Rails 8 uses Importmap for JavaScript management:
- Check `config/importmap.rb`
- No bundling required
- Native ES6 modules support

### Solid Cache/Queue/Cable
Rails 8 includes database-backed adapters:
```cmd
rails solid_cache:install
rails solid_queue:install
rails solid_cable:install
```

### Kamal Deployment
Rails 8 includes Kamal for deployment:
- Check `config/deploy.yml`
- Docker-based deployment ready

## Testing Rails Features

### 1. Database Operations
```cmd
# Generate a model
rails generate model User name:string email:string

# Run migration
rails db:migrate

# Test in console
rails console
# In console:
User.create(name: "Test User", email: "test@example.com")
User.all
```

### 2. Asset Pipeline Test
Create `app/assets/stylesheets/custom.css`:
```css
body {
  background-color: #f0f0f0;
  font-family: Arial, sans-serif;
}
```

### 3. JavaScript Test
Create `app/javascript/controllers/hello_controller.js`:
```javascript
import { Controller } from "@hotwired/stimulus"

export default class extends Controller {
  connect() {
    this.element.textContent = "Hello World from Stimulus!"
  }
}
```

## Common Windows Issues and Solutions

### Issue 1: SQLite3 Native Extension Error
```
LoadError: cannot load such file -- sqlite3/sqlite3_native
```

**Solution:**
```cmd
# Reinstall SQLite3 development tools
ridk exec pacman -S mingw-w64-ucrt-x86_64-sqlite3

# Or use alternative database
# Add to Gemfile:
gem 'pg' # for PostgreSQL
```

### Issue 2: SSL Certificate Issues
```cmd
# If gem install fails with SSL errors
gem sources --remove https://rubygems.org/
gem sources --add https://rubygems.org/
```

### Issue 3: Native Gem Compilation
```cmd
# Ensure development tools are installed
ridk install
# Choose option 3: MSYS2 and MINGW development toolchain
```

### Issue 4: Path Issues
- Ensure Ruby, Node.js, and Git are in Windows PATH
- Restart command prompt after PATH changes
- Use `where ruby` to verify Ruby location

## Performance Optimization

### Bootsnap Configuration
Ensure Bootsnap is enabled in `config/boot.rb`:
```ruby
require "bootsnap/setup"
```

### Bundle Configuration
```cmd
bundle config set --local path 'vendor/bundle'
bundle config set --local jobs 4
```

### Development Server Optimization
In `config/environments/development.rb`:
```ruby
config.cache_classes = false
config.eager_load = false
config.file_watcher = ActiveSupport::EventedFileUpdateChecker
```

## Rails 8 vs Previous Versions

### Key Differences:
1. **Propshaft**: Replaces Sprockets for simpler asset handling
2. **Importmap**: Native ES6 imports without bundling
3. **Solid adapters**: Database-backed cache, queue, and cable
4. **Kamal**: Built-in deployment solution
5. **Thruster**: HTTP acceleration for Puma
6. **Updated dependencies**: Latest Ruby 3.4, modern JavaScript

### Migration Notes:
- Existing Rails apps can be upgraded gradually
- Asset pipeline changes may require updates
- JavaScript packaging simplified
- Database adapters more flexible

## Troubleshooting Commands

```cmd
# Check Rails environment
rails about

# Check gem dependencies
bundle list
bundle outdated

# Clear caches
rails tmp:clear
bundle clean

# Reinstall gems
rm Gemfile.lock
bundle install

# Check for security issues
bundle audit

# Code quality checks
rubocop
brakeman
```

## Production Deployment Checklist

- [ ] Database configured (PostgreSQL recommended)
- [ ] Environment variables set
- [ ] SSL certificates configured
- [ ] Asset precompilation tested
- [ ] Background job processing setup
- [ ] Monitoring and logging configured
- [ ] Backup strategy implemented
- [ ] Security scan completed

## Resources and Documentation

- **Rails Guides**: https://guides.rubyonrails.org/
- **Rails 8 Release Notes**: https://guides.rubyonrails.org/8_0_release_notes.html
- **Ruby Documentation**: https://ruby-doc.org/
- **Propshaft**: https://github.com/rails/propshaft
- **Importmap**: https://github.com/rails/importmap-rails
- **Kamal**: https://kamal-deploy.org/
- **Community**: https://discuss.rubyonrails.org/

---

This verification script confirms that Ruby on Rails 8 is properly installed and configured on your Windows system with all necessary dependencies and tools.
