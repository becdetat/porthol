# Porthol

(it's Welsh for Portal)

This uses:
- ASP.NET Core
- .NET 7
- SQLite
- Vue
- TypeScript

Rather than use Vite to serve the frontend, we're using it to build the frontend statically (in a watch loop) and write it to the `wwwroot` folder within the API. The API then serves the frontend statically from the root request path. This keeps things simple - a single server, a consistent production deploy story, and no annoying CSRF issues in development.

Eventually this will be released as a Docker container, with a configurable volume for the SQLite database.


## Initial setup and getting running
### Frontend
1. `cd frontend`
2. `npm install`
3. `npm run build-and-watch`

The `npm run build-and-watch` command enters a watch loop which builds the frontend and writes it to `../backend/wwwroot` (which is served by the API).

### Backend
1. `cd backend/Api`
2. `dotnet run`

The API serves the `wwwroot` folder using `app.UseDefaultFiles(); app.UseStaticFiles();` in `Program.cs`.


## EF Migrations
It uses a SQLite database, currently just stored at `backend/Domain/portal.db`. This will need to be moved to somewhere where it can be treated as a volume by Docker, for persistence across docker compose runs.

Install the EF tooling: `dotnet tool install --global dotnet-ef`

To update after a model change:

```
dotnet ef migrations add <name-of-migration>
dotnet ef database update
```

