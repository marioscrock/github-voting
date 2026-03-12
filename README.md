# 🗳️ GitHub Comment Reactions Analyzer

A static web app that analyzes GitHub comment reactions and detects voting conflicts. Paste a GitHub comment link, and it will fetch all reactions, map each user to their assigned entity (country, company, team, etc.), and alert on conflicts.

No backend required — uses the public GitHub API and runs entirely in the browser.

## Features

✅ **Parse GitHub Comments** - Paste any GitHub issue comment link  
✅ **Public API** - No authentication required  
✅ **Flexible Entity Mapping** - Map users to countries, companies, teams, or any label  
✅ **Flag Display** - Automatically renders flag emojis for ISO country codes  
✅ **Conflict Detection** - Alerts when multiple users share the same entity  
✅ **Unknown User Alerts** - Warns when a reacting user is not in the voter list  
✅ **Live Statistics** - Thumbs up/down counts and duplicate totals  
✅ **Color-Coded Display** - Red rows for conflicts, yellow for unregistered users  

## Setup

### 1. Create your voter list

Copy the example file and fill in your participants:

```bash
cp voting_data.example.csv voting_data.csv
```

Edit `voting_data.csv` with your GitHub usernames and their assigned entity:

```csv
GitHub Name,Voting for
alice,CompanyA
bob,CompanyB
carol,CompanyA
```

Entities can be:
- **ISO country codes** (SI, NL, DE, …) → displayed with flag emoji automatically
- **Any text** (company name, team, group) → displayed as plain text

### 2. Serve locally

`voting_data.csv` is loaded via `fetch()`, so the page must be served over HTTP (not opened as `file://`).

**Python 3:**
```bash
python -m http.server 8000
```
Then open: **http://localhost:8000/voting-validator.html**

**Node.js (via npx):**
```bash
npx http-server
```

### 3. Deploy to GitHub Pages

> ⚠️ `voting_data.csv` is in `.gitignore` by default to protect participant data.  
> Add it to your push only if the data is safe to make public.

```bash
git init
git add voting-validator.html voting_data.example.csv .gitignore README.md
# only include voting_data.csv if you want it public:
# git add voting_data.csv
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/YOUR-REPO
git push -u origin main
```

Enable GitHub Pages in repository **Settings → Pages → Deploy from main branch**.

App will be live at: `https://YOUR-USERNAME.github.io/YOUR-REPO/voting-validator.html`

## How to Use

1. **Get a GitHub Comment Link**
   - Open any GitHub issue, scroll to a comment
   - Click the **"..."** menu on the comment → **"Copy link"**
   - Example: `https://github.com/owner/repo/issues/123#issuecomment-456789`

2. **Paste & Analyze**
   - Paste the link into the input field and click **🔍 Analyze**

3. **Read the Results**
   - **✅ Green Alert:** No conflicts, all users registered
   - **⚠️ Yellow Alert:** One or more reacting users are not in `voting_data.csv`
   - **⛔ Red Alert:** Multiple users from the same entity reacted

## File Structure

```
├── voting-validator.html       # Main application (open in browser)
├── voting_data.example.csv    # Template — copy to voting_data.csv and fill in
├── voting_data.csv            # Your actual voter list (gitignored)
├── .gitignore
└── README.md
```

## Supported Country Flags

Any two-letter ISO 3166-1 alpha-2 code that maps to a flag emoji will render automatically. Pre-configured ones:

| Code | Country | Flag |
|------|---------|------|
| SI | Slovenia | 🇸🇮 |
| NL | Netherlands | 🇳🇱 |
| AT | Austria | 🇦🇹 |
| DK | Denmark | 🇩🇰 |
| SE | Sweden | 🇸🇪 |
| BE | Belgium | 🇧🇪 |
| DE | Germany | 🇩🇪 |
| IT | Italy | 🇮🇹 |
| UK | United Kingdom | 🇬🇧 |
| CZ | Czech Republic | 🇨🇿 |
| CH | Switzerland | 🇨🇭 |
| NO | Norway | 🇳🇴 |

To add more, extend the `COUNTRY_FLAGS` object at the top of `voting-validator.html`.

## API Notes

- **Rate Limit:** 60 requests/hour per IP (unauthenticated public API)
- **Works with:** Public repositories only
- **Reactions per request:** Up to 100 (pagination not implemented)

## Troubleshooting

**CSV not loading / "no data" message**
- Open via `http://localhost:8000` — not `file://` directly
- Verify `voting_data.csv` exists in the same directory as the HTML file
- Check the browser console (F12) for fetch errors

**No reactions found**
- Confirm the comment URL includes `#issuecomment-XXXXXXXXX`
- Make sure the repository is public

**Rate limit hit (403)**
- Wait an hour, or add a GitHub token to the request headers

---

**Made with ❤️ for voting transparency**
