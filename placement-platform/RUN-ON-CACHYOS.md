# Run the project on CachyOS

Open a terminal in the project folder (`placement-platform`). You need Docker, two configured Supabase projects (or one shared project), and a Gemini API key for AI features.

## 1. Install and start Docker

```bash
sudo pacman -Syu docker docker-compose
sudo systemctl enable --now docker
sudo docker run --rm hello-world
```

## 2. Set up environment files

Create the three files if you do not already have them. These commands keep existing files intact:

```bash
test -f interview-repository/backend/.env || cp interview-repository/backend/.env.example interview-repository/backend/.env
test -f interview-repository/frontend/.env || cp interview-repository/frontend/.env.example interview-repository/frontend/.env
test -f Placement_Intelligence_Platform/.env || cp Placement_Intelligence_Platform/.env.example Placement_Intelligence_Platform/.env
```

Edit each `.env` and replace the example values with your Supabase and Gemini credentials. Set `AI_SERVICE_API_KEY` in `interview-repository/backend/.env` to the same random secret as `INTERNAL_API_KEY` in `Placement_Intelligence_Platform/.env`. **Do not commit these files.** The variable-by-variable guide is in the root `README.md` under “Create the three `.env` files”.

Before the first run, create the tables in Supabase B by running `Placement_Intelligence_Platform/sql/master_schema.sql` and then `preparation_schema.sql` in its SQL editor. Backend A creates its tables on startup.

## 3. Build and run the whole app

From the project folder:

```bash
sudo docker compose up --build
```

Wait for the containers to finish starting, then open **http://localhost:5173**. The backend is at **http://localhost:8080**. Keep this terminal open to see logs; press `Ctrl+C` to stop.

To start in the background instead, add `-d`:

```bash
sudo docker compose up --build -d
sudo docker compose logs -f
```

Stop and remove the containers with:

```bash
sudo docker compose down
```

If Docker reports a permission error, keep `sudo` before each `docker` command as shown. Compose uses the CachyOS/Arch `docker-compose` package and the repository’s root `docker-compose.yml`.
