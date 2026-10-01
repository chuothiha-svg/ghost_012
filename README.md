# Gọn Link

The Cloudflare Worker serves the frontend and handles redirects; D1 stores short links persistently. It does not use a third-party shortening service. Cloudflare keeps it running when your computer is off and provides a public `workers.dev` address without requiring a custom domain.

You can choose a 3–32 character alias or use a random 12-character code. Rename controls are available in the browser that created a link; a private owner token is stored there. Renaming invalidates the old code. The destination is omitted from browser history and local storage, but becomes visible after a recipient follows the redirect.

## Deploy from GitHub

GitHub Pages alone cannot run the redirect API or database. This project uses a GitHub Actions workflow to deploy the Worker to Cloudflare whenever you push to `main`.

1. Install Node.js, open PowerShell in this folder, and run `npm install` then `npx wrangler login`.
2. Create the Cloudflare database with `npm run db:create` and copy its `database_id`.
3. In the GitHub repository settings, add Actions secrets named `CLOUDFLARE_API_TOKEN`, `CLOUDFLARE_ACCOUNT_ID`, and `D1_DATABASE_ID`. The API token needs permission to deploy Workers and manage D1.
4. Create an empty repository on GitHub, then run these commands here (replace the URL):

	```powershell
	git init -b main
	git add .
	git commit -m "Initial Gọn Link app"
	git remote add origin https://github.com/YOUR_NAME/YOUR_REPOSITORY.git
	git push -u origin main
	```

	The workflow initializes the D1 schema and deploys the app after the push.

The first successful deploy creates a public `https://gon-link.<your-account>.workers.dev` address. Share that address and links made through it; Cloudflare keeps the service available when your computer is off. You only need another push to deploy later changes.

## Run locally

```powershell
python server.py
```

Open `http://localhost:8000`. Local links only work on this computer. The local Python server uses `links.sqlite3`, which is ignored by Git and separate from the deployed D1 database.

## Custom domain

To use `https://ghost-012.com/<code>`, register the domain and attach it to the deployed Worker in Cloudflare. Until DNS and the domain are configured, use the `workers.dev` address printed by Wrangler.
