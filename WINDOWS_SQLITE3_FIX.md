# Windows SQLite3 Fix for Rails 8 and Ruby 3.4

## Problem Description

When using Ruby 3.4 with Rails 8 on Windows, you may encounter this error:

```
LoadError: cannot load such file -- sqlite3/sqlite3_native
LoadError: 127: The specified procedure could not be found. - sqlite3_native.so
```

This is a known compatibility issue between the SQLite3 gem and Ruby 3.4 on Windows.

## Root Cause

The issue occurs because:
1. The precompiled SQLite3 gem for Windows may not be compatible with Ruby 3.4
2. The native extension requires specific Windows runtime libraries
3. The MSYS2 toolchain may need updates

## Solution Steps

### Step 1: Install/Update MSYS2 Development Tools

```cmd
# Run this command to install/update MSYS2 development tools
ridk install

# Choose option 3: MSYS2 and MINGW development toolchain
# This ensures you have the latest compilation tools
```

### Step 2: Install SQLite3 Development Libraries

```cmd
# Install SQLite3 development libraries through MSYS2
ridk exec pacman -S mingw-w64-ucrt-x86_64-sqlite3
ridk exec pacman -S mingw-w64-ucrt-x86_64-sqlite3-dev
```

### Step 3: Alternative Solutions

#### Option A: Use PostgreSQL Instead
If SQLite3 continues to cause issues, switch to PostgreSQL:

1. Install PostgreSQL for Windows from https://www.postgresql.org/download/windows/
2. Update your Gemfile:
```ruby
# Comment out sqlite3
# gem "sqlite3", "~> 2.1"

# Add PostgreSQL
gem "pg", "~> 1.1"
```

3. Update `config/database.yml`:
```yaml
default: &default
  adapter: postgresql
  encoding: unicode
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>

development:
  <<: *default
  database: myapp_development
  username: postgres
  password: your_password
  host: localhost

test:
  <<: *default
  database: myapp_test
  username: postgres
  password: your_password
  host: localhost
```

#### Option B: Force Compile SQLite3 from Source
```cmd
# Uninstall existing sqlite3 gem
gem uninstall sqlite3 --force

# Install from source with native compilation
gem install sqlite3 --platform=ruby --source=https://rubygems.org

# Or install specific version
gem install sqlite3 -v 1.7.3 --platform=ruby
```

#### Option C: Use Older SQLite3 Version
```ruby
# In your Gemfile, specify an older, more stable version
gem "sqlite3", "~> 1.7.0"
```

### Step 4: Verify Installation

After applying any of the above solutions:

```cmd
# Navigate to your Rails app
cd your_rails_app

# Reinstall dependencies
bundle install

# Test database connection
rails db:create
rails db:migrate

# Start server
rails server
```

## Prevention for New Projects

When creating new Rails applications, you can avoid this issue:

### Option 1: Use PostgreSQL from Start
```cmd
rails new myapp --database=postgresql
```

### Option 2: Skip SQLite3 Setup
```cmd
rails new myapp --skip-active-record
# Then add your preferred database later
```

### Option 3: Specify SQLite3 Version
```cmd
rails new myapp
cd myapp
# Edit Gemfile to specify sqlite3 version before bundle install
```

## Windows-Specific Considerations

### Environment Variables
Ensure these are properly set:
- `PATH` includes Ruby, MSYS2, and PostgreSQL (if used)
- `RAILS_ENV=development` (default)

### Antivirus Software
Some antivirus software may interfere with gem compilation:
- Add Ruby installation directory to antivirus exceptions
- Temporarily disable real-time protection during gem installation

### Windows Defender
Add exclusions for:
- `C:\Users\YourUser\Ruby34-x64\`
- Your Rails project directories
- MSYS2 installation directory

## Testing Your Fix

Create a simple test to verify SQLite3 is working:

```ruby
# test_sqlite.rb
require 'sqlite3'

begin
  # Create a test database
  db = SQLite3::Database.new("test.db")
  
  # Create a test table
  db.execute <<-SQL
    CREATE TABLE IF NOT EXISTS test (
      id INTEGER PRIMARY KEY,
      name TEXT
    );
  SQL
  
  # Insert test data
  db.execute("INSERT INTO test (name) VALUES (?)", ["Test User"])
  
  # Query data
  rows = db.execute("SELECT * FROM test")
  puts "SQLite3 is working! Found #{rows.length} rows"
  
  # Clean up
  db.close
  File.delete("test.db") if File.exist?("test.db")
  
  puts "✅ SQLite3 test passed"
rescue => e
  puts "❌ SQLite3 test failed: #{e.message}"
  puts "Consider using PostgreSQL instead"
end
```

Run the test:
```cmd
ruby test_sqlite.rb
```

## When All Else Fails

If SQLite3 continues to cause problems:

1. **Use PostgreSQL**: More reliable on Windows and better for production
2. **Use MySQL**: Another solid alternative with good Windows support
3. **Use Docker**: Run Rails in a Linux container for consistency
4. **Use WSL2**: Windows Subsystem for Linux provides a Linux environment

## PostgreSQL Setup Guide

Since PostgreSQL is more reliable on Windows:

### Install PostgreSQL
1. Download from https://www.postgresql.org/download/windows/
2. Run installer with default settings
3. Remember the superuser password
4. Add PostgreSQL bin to PATH

### Configure Rails for PostgreSQL
```ruby
# Gemfile
gem "pg", "~> 1.1"

# Remove or comment out
# gem "sqlite3"
```

```yaml
# config/database.yml
default: &default
  adapter: postgresql
  encoding: unicode
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>

development:
  <<: *default
  database: myapp_development
  username: postgres
  password: <%= ENV["POSTGRES_PASSWORD"] %>
  host: localhost

test:
  <<: *default
  database: myapp_test
  username: postgres
  password: <%= ENV["POSTGRES_PASSWORD"] %>
  host: localhost
```

### Set Environment Variable
```cmd
# Set PostgreSQL password
set POSTGRES_PASSWORD=your_password

# Or create .env file in your Rails root:
# POSTGRES_PASSWORD=your_password
```

## Summary

The SQLite3 issue with Ruby 3.4 on Windows is solvable, but PostgreSQL provides a more robust solution for Rails development on Windows. Choose the approach that best fits your project needs and technical requirements.
