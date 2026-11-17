# Blog Workflow

Live site: https://duncip.github.io/

Every change has two steps:

| Branch     | Holds                       | Updated by                |
|------------|-----------------------------|---------------------------|
| `main`     | Source files (`.qmd`, images, config) | `git push`      |
| `gh-pages` | Rendered HTML (what the site serves)  | `quarto publish gh-pages` |

Pushing to `main` alone does **not** change the live site.

---

## Quick reference

```powershell
quarto preview                          # 1. preview locally (Ctrl+C to stop)
git add .                               # 2. save source
git commit -m "Describe the change"
git push
quarto publish gh-pages                 # 3. update the live site
```

---

## Creating a new post

1. Make a folder under `posts/` with an `index.qmd` inside:

   ```
   posts/my-new-post/index.qmd
   ```

2. Start the file with front matter:

   ```yaml
   ---
   title: "My New Post"
   date: "2026-11-17"
   categories: [python, analysis]
   image: thumbnail.jpg      # optional; shown on the listing page
   draft: true               # remove when ready to publish
   ---

   Your content here.
   ```

3. Put images for the post in the same folder and reference them by name:
   `![Caption](chart.png)`

4. While `draft: true` is set, the post won't appear on the live site.
   Delete that line when it's ready.

---

## Editing existing pages

- **Home page listing:** `index.qmd`
- **About page:** `about.qmd`
- **Navbar, site title, theme, links:** `_quarto.yml`
- **Custom styles:** `styles.css`
- **Defaults for all posts:** `posts/_metadata.yml`

Then run the same preview → push → publish steps.

---

## Recommended setting: freeze

Add this to `_quarto.yml` so posts with Python code only re-run when that
post changes:

```yaml
execute:
  freeze: auto
```

Commit the `_freeze/` folder it creates. It speeds up renders, and old posts
won't break if a package changes.

---

## Gotchas

- **`requirements.txt` encoding:** in PowerShell, don't use
  `pip freeze > requirements.txt` (it writes UTF-16). Use:
  ```powershell
  pip freeze | Out-File -Encoding utf8 requirements.txt
  ```
- **Don't edit `gh-pages` by hand.** Quarto overwrites it on every publish.
- **Don't commit `_site/` or `.quarto/`.** They're in `.gitignore`.
- **Push rejected?** Run `git pull` first, then `git push`.
- **OneDrive:** if git gives odd file-lock errors, pause OneDrive syncing,
  or move the repo outside OneDrive (e.g. `C:\Users\dunca\code\`).
