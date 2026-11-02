# FactoryBridge

A Ruby on Rails starter application using PostgreSQL for the database and RSpec for testing.

## Technology Stack

- **Ruby:** 4.0.9
- **Rails:** 8.1.4
- **Database:** PostgreSQL
- **Testing:** RSpec Rails 8.x
- **Ruby version manager:** RVM

## Prerequisites

Install the following before setting up the application:

- Git
- Ruby 4.0.9
- Bundler
- PostgreSQL
- Homebrew and Xcode Command Line Tools if using macOS

The project's `.ruby-version` and `Gemfile.lock` files define the Ruby and gem versions to use.

## Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/dbhanushali/factory_bridge.git factory_bridge
cd factory_bridge
```

### 2. Select the Ruby Version

If using RVM:

```bash
rvm use ruby-4.0.9
```

Verify the active version:

```bash
ruby -v
```

### 3. Install Dependencies

```bash
bundle install
```

If Bundler is missing, install the version listed under `BUNDLED WITH` in `Gemfile.lock`, then retry.

```bash
gem install bundler -v VERSION
bundle install
```

Replace `VERSION` with the version recorded in the lockfile.

### 4. Start PostgreSQL

If PostgreSQL is already installed and running, skip this step.

On macOS with Homebrew:

```bash
brew install postgresql@17
brew services start postgresql@17
```

Check the service status:

```bash
brew services list
```

### 5. Configure the Database

Review:

```text
config/database.yml
```

Ensure the development and test settings match your local PostgreSQL installation.

- PostgreSQL must be running.
- The configured database role must be able to create development and test databases.
- Keep passwords out of committed files.
- Use environment variables when credentials are required.

> Rails does not automatically load `.env` files unless an environment-file loader is configured.

### 6. Prepare the Databases

```bash
bin/rails db:prepare
RAILS_ENV=test bin/rails db:prepare
```

These commands create the configured databases when necessary and apply available migrations.

### 7. Start the Application

```bash
bin/rails server
```

Open **http://localhost:3000** in your browser.

Press **Control+C** to stop the server.

## Running Tests

Run the complete RSpec suite:

```bash
bundle exec rspec
```

A blank application without specifications may report:

```text
0 examples, 0 failures
```

This confirms that the runner starts. It does not indicate application test coverage.

### Run a Specific Spec

Replace the example path with an existing spec file:

```bash
bundle exec rspec spec/models/example_spec.rb
```

### Run a Specific Example

```bash
bundle exec rspec spec/models/example_spec.rb:10
```

### RSpec Configuration

- `.rspec` — default command-line options.
- `spec/spec_helper.rb` — general RSpec configuration.
- `spec/rails_helper.rb` — Rails integration and test database setup.

For specifications that need Rails, include:

```ruby
require "rails_helper"
```

## Common Commands

| Task | Command |
| --- | --- |
| Check Ruby version | `ruby -v` |
| Check Rails version | `bin/rails --version` |
| Start the server | `bin/rails server` |
| Open Rails console | `bin/rails console` |
| Open database console | `bin/rails dbconsole` |
| List application routes | `bin/rails routes` |
| Prepare the database | `bin/rails db:prepare` |
| Run migrations | `bin/rails db:migrate` |
| Prepare the test database | `RAILS_ENV=test bin/rails db:prepare` |
| Run RSpec | `bundle exec rspec` |

## Project Structure

| Path | Purpose |
| --- | --- |
| `app/` | Application models, controllers, views, and related code |
| `config/` | Routes, environments, initializers, and database configuration |
| `db/` | Migrations, database schema, and seed data |
| `spec/` | RSpec specifications and test helpers |
| `Gemfile` | Application dependencies |
| `Gemfile.lock` | Resolved dependency versions |
| `.ruby-version` | Project Ruby version |
| `.github/workflows/` | GitHub Actions workflows, if included |

## Credentials and Environment Variables

Never commit:

- Database passwords.
- API keys or access tokens.
- Rails credential decryption keys.
- Private keys.
- Production data.
- Environment files containing secrets.

Ensure `.gitignore` includes these entries alongside the generated Rails rules:

```gitignore
/config/master.key
/config/credentials/*.key
.env
.env.*
!.env.example
```

An `.env.example` file should contain placeholders only.

Encrypted files such as `config/credentials.yml.enc` can be committed. Their corresponding decryption keys must remain private.

If the application requires existing encrypted credentials, obtain the matching key securely from the maintainer.

> Adding a file to `.gitignore` does not remove it from existing commits or Git history.

Commit `.ruby-version` and `Gemfile.lock` so other developers can reproduce the project environment.

## Development Workflow

### 1. Create a Branch

```bash
git switch -c feature/your-feature-name
```

### 2. Implement Your Changes

- Add or update application code.
- Add relevant RSpec specifications.
- Create database migrations when required.
- Update documentation when behavior or setup changes.

### 3. Run Tests

```bash
RAILS_ENV=test bin/rails db:prepare
bundle exec rspec
```

### 4. Review and Commit

```bash
git status
git add .
git diff --cached
git commit -m "Describe your changes"
```

### 5. Push Your Branch

```bash
git push -u origin feature/your-feature-name
```

Open a pull request on GitHub to review and merge the changes.

## Continuous Integration

Review any generated GitHub Actions workflows before relying on them.

The test workflow should:

- Install the project's Ruby version.
- Install dependencies using Bundler.
- Start a PostgreSQL service.
- Configure the test database connection.
- Prepare the test database.
- Run `bundle exec rspec`.

> A workflow that runs only `bin/rails test` does not run the RSpec suite.

## Troubleshooting

### PostgreSQL Connection Refused

Check that PostgreSQL is running:

```bash
brew services list
```

If using the Homebrew PostgreSQL 17 installation:

```bash
brew services start postgresql@17
```

Confirm that the host, port, or socket configuration in `config/database.yml` matches your server.

### PostgreSQL Role Does Not Exist

Configure Rails to use an existing PostgreSQL role, or create a local development role using your PostgreSQL administrator account.

The development role must have permission to create the application's development and test databases.

### Database Missing or Migrations Pending

```bash
bin/rails db:prepare
RAILS_ENV=test bin/rails db:prepare
```

### Wrong Ruby Version

From the project directory:

```bash
rvm use ruby-4.0.9
ruby -v
which ruby
```

### OpenSSL Fails to Load

Check Ruby's OpenSSL support:

```bash
ruby -ropenssl -e 'puts OpenSSL::OPENSSL_VERSION'
```

If Ruby references a missing OpenSSL library, repair or rebuild that Ruby installation against a supported installed library.

Do not symlink OpenSSL 3 libraries to OpenSSL 1.1 filenames.

## Deployment

Deployment instructions will be added when a hosting environment is selected.

Before deploying, configure:

- Production PostgreSQL.
- Production credentials and environment variables.
- Database migrations.
- Asset compilation and serving.
- Any enabled background job services.
- Application logging and monitoring.

Publishing this repository on GitHub does not deploy the application.

## License

A license has not yet been specified.

Add a `LICENSE` file before distributing this project under an open-source license. A public GitHub repository alone does not grant an open-source license.
