# Ruby on Rails 8 Installation Guide for Windows

## Current Installation Status ✅

Your system already has a complete Rails 8 development environment installed:

- **Ruby**: 3.4.4 (2025-05-14 revision a38531fd3f) +PRISM [x64-mingw-ucrt]
- **Rails**: 8.0.2
- **Node.js**: v22.15.0
- **Yarn**: 1.22.22
- **Bundler**: 2.6.9
- **Git**: 2.49.0.windows.1
- **SQLite**: 3.50.0

## Complete Installation Guide (For Fresh Windows Setup)

### Phase 1: System Prerequisites

#### 1.1 Windows Updates and Prerequisites
```powershell
# Ensure Windows is updated
# Open Windows Update in Settings and install all updates
```

#### 1.2 Install Git for Windows
1. Download from: https://git-scm.com/download/win
2. Install with recommended settings
3. Choose "Use Git from the Windows Command Prompt"
4. Configure:
```cmd
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
git config --global core.autocrlf true
```

### Phase 2: Ruby Installation

#### 2.1 Download RubyInstaller with DevKit
1. Visit: https://rubyinstaller.org/downloads/
2. Download **Ruby+Devkit 3.4.4-1 (x64)** - the version with DEVKIT
3. Run installer as Administrator
4. Select "Add Ruby executables to your PATH"
5. Install MSYS2 development toolchain when prompted

#### 2.2 Verify Ruby Installation
```cmd
ruby --version
# Should show: ruby 3.4.4 (2025-05-14 revision a38531fd3f) +PRISM [x64-mingw-ucrt]

gem --version
# Should show gem version

bundler --version
# Should show bundler version
```

#### 2.3 Update RubyGems and Install Bundler
```cmd
gem update --system
gem install bundler
```

### Phase 3: Node.js and JavaScript Runtime

#### 3.1 Install Node.js
1. Visit: https://nodejs.org/
2. Download LTS version (recommended)
3. Run installer with default settings
4. Verify installation:
```cmd
node --version
# Should show v20.x or later

npm --version
# Should show npm version
```

#### 3.2 Install Yarn
```cmd
npm install -g yarn
yarn --version
# Should show yarn version
```

### Phase 4: Database Setup

#### 4.1 SQLite (Default - Already included with Ruby)
```cmd
sqlite3 --version
# Should show SQLite version
```

#### 4.2 PostgreSQL (Optional - For production-like development)
1. Download from: https://www.postgresql.org/download/windows/
2. Install with default settings
3. Remember the superuser password
4. Add PostgreSQL bin directory to PATH
5. Install PostgreSQL gem:
```cmd
gem install pg
```

### Phase 5: Rails Installation

#### 5.1 Install Rails 8
```cmd
gem install rails
```

#### 5.2 Verify Rails Installation
```cmd
rails --version
# Should show Rails 8.x
```

### Phase 6: Development Environment Configuration

#### 6.1 Configure Environment Variables
Add to Windows Environment Variables:
- Ensure Ruby, Node.js, and Git are in PATH
- Set RAILS_ENV=development (optional, default)

#### 6.2 Install Essential Development Tools
```cmd
# Install useful gems globally
gem install debug
gem install rubocop
gem install brakeman
```

### Phase 7: Create and Test a Rails Application

#### 7.1 Generate New Application
```cmd
rails new my_test_app --database=sqlite3
cd my_test_app
```

#### 7.2 Install Dependencies
```cmd
bundle install
```

#### 7.3 Test the Application
```cmd
rails server
# Open browser to http://localhost:3000
```

#### 7.4 Test Database Operations
```cmd
rails generate model User name:string email:string
rails db:migrate
rails console
# In console: User.create(name: "Test", email: "test@example.com")
```

## Verification Checklist

- [ ] Ruby 3.4.4+ installed and accessible
- [ ] Rails 8.x installed and working
- [ ] Node.js LTS installed with npm
- [ ] Yarn package manager working
- [ ] Git configured and working
- [ ] SQLite database accessible
- [ ] Can generate new Rails applications
- [ ] Development server starts successfully
- [ ] Database migrations work
- [ ] Asset pipeline compiles correctly
- [ ] JavaScript integration working (Stimulus/Turbo)

## Common Windows-Specific Issues and Solutions

### SSL Certificate Issues
```cmd
# If gem install fails with SSL errors:
gem sources --remove https://rubygems.org/
gem sources --add http://rubygems.org/
# Then switch back to HTTPS after installing
```

### Native Gem Compilation Issues
```cmd
# Ensure MSYS2 development tools are installed:
ridk install
# Choose option 3 (MSYS2 and MINGW development toolchain)
```

### Path Issues
- Ensure Ruby, Node.js, and Git directories are in Windows PATH
- Restart command prompt after PATH changes
- Use full paths if needed

### File Permission Issues
- Run command prompt as Administrator for global installations
- Use `--user-install` flag for gem installs if needed

### Line Ending Issues
```cmd
# Configure Git for Windows line endings:
git config --global core.autocrlf true
```

## Performance Optimization

### Bootsnap Configuration
Add to Gemfile:
```ruby
gem 'bootsnap', require: false
```

### Bundle Configuration
```cmd
bundle config set --local path 'vendor/bundle'
bundle config set --local cache_path 'vendor/cache'
```

### Yarn Configuration
```cmd
yarn config set cache-folder .yarn-cache
```

## Rails 8 Specific Features

### New in Rails 8
- **Propshaft**: New asset pipeline (replaces Sprockets)
- **Importmap**: Native ES6 imports without bundling
- **Solid Cache/Queue/Cable**: Database-backed adapters
- **Kamal**: Built-in deployment tool
- **Thruster**: HTTP acceleration for Puma

### Key Configuration Files
- `config/importmap.rb` - JavaScript import maps
- `app/assets/config/manifest.js` - Asset pipeline
- `config/deploy.yml` - Kamal deployment
- `Procfile.dev` - Development processes

## Troubleshooting

### Common Commands for Debugging
```cmd
# Check Ruby/Rails environment
ruby -v
rails -v
bundle env

# Check gem dependencies
bundle list
bundle outdated

# Clear caches
rails tmp:clear
bundle clean

# Reinstall gems
bundle install --force
```

### Getting Help
- Rails Guides: https://guides.rubyonrails.org/
- Ruby documentation: https://ruby-doc.org/
- Community: https://discuss.rubyonrails.org/
- Stack Overflow: Use tags `ruby-on-rails`, `windows`
