# CdGigalixir

# CD Gigalixir Project

## Project Overview

This is a Continuous Deployment (CD) project built with:
- Elixir 1.17.3
- Phoenix 1.7.20
- Deployed on Gigalixir

## Prerequisites

- Elixir 1.17.3
- Erlang/OTP 26
- PostgreSQL 14+
- Git

## Project Setup

### 1. Clone the Repository
```bash
git clone https://github.com/your-username/cd-gigalixir.git
cd cd-gigalixir
```

### 2. Install Dependencies
```bash
mix deps.get
mix deps.compile
```

### 3. Database Configuration

#### 3.1 Create PostgreSQL Database
```bash
# Connect to PostgreSQL
sudo -u postgres psql

# Inside PostgreSQL console
CREATE DATABASE cd_gigalixir_dev;
CREATE USER your_username WITH PASSWORD 'your_password';
GRANT ALL PRIVILEGES ON DATABASE cd_gigalixir_dev TO your_username;
\q
```

#### 3.2 Configure Database Credentials
Edit `config/dev.exs`:
```elixir
config :cd_gigalixir, CdGigalixir.Repo,
  username: "your_username",
  password: "your_password",
  database: "cd_gigalixir_dev",
  hostname: "localhost",
  port: 5432
```

### 4. Create Categories CRUD
```bash
# Generate Live Views for Categories
mix phx.gen.live Categories Category categories name:string description:text

# Run migrations
mix ecto.create
mix ecto.migrate
```

### 5. Add Routes
Add the following to `lib/cd_gigalixir_web/router.ex` inside the browser scope:
```elixir
live "/categories", CategoryLive.Index, :index
live "/categories/new", CategoryLive.Index, :new
live "/categories/:id/edit", CategoryLive.Index, :edit
live "/categories/:id", CategoryLive.Show, :show
live "/categories/:id/show/edit", CategoryLive.Show, :edit
```

### 6. Run the Application
```bash
mix phx.server
```

Visit `http://localhost:4000/categories` to see your new CRUD interface.

## Continuous Deployment with Gigalixir and GitHub Actions

### GitHub Secrets Configuration
Configure the following secrets in your GitHub repository settings:
- `GIGALIXIR_USERNAME`: Your Gigalixir username
- `GIGALIXIR_PASSWORD`: Your Gigalixir password
- `GIGALIXIR_APP`: Your Gigalixir application name
- `SSH_PRIVATE_KEY`: Your SSH private key for deployment

### Deployment Workflow
The `.github/workflows/deploy.yml` file is configured to automatically deploy to Gigalixir when changes are pushed to the `main` branch.

## Development Tips
- Use `mix format` to keep your code consistently formatted
- Run tests with `mix test`
- Check code quality with `mix credo`

## Contributing
1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## License
Distributed under the MIT License. See `LICENSE` for more information.