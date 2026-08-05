# ShiftScope GitHub Pages

Static schedule OCR reviewer for weekly staff schedules.

## What it does

- Upload a schedule screenshot or photo in the browser.
- Runs OCR client-side with Tesseract.js.
- Shows the original image, a processed OCR canvas, raw OCR text, and a cleaner schedule breakdown.
- Includes filters for day and role.
- Works on GitHub Pages because it is a static HTML app.

## Files

- `index.html` — main app for GitHub Pages.
- `README.md` — setup and deployment instructions.
- `.gitignore` — basic ignore file for local clutter.

## Quick start

1. Create a new GitHub repository.
2. Upload all files from this folder to the repository root.
3. Commit and push.
4. In GitHub, open **Settings > Pages**.
5. Under **Build and deployment**, choose **Deploy from a branch**.
6. Select your main branch and the `/ (root)` folder.
7. Save the settings.
8. Wait for GitHub Pages to publish the site.

GitHub Pages serves static HTML, CSS, and JavaScript, which matches how this app is built.[cite:3]

## Recommended repo structure

```text
shiftscope-github-pages/
├── index.html
├── README.md
└── .gitignore
```

## How to use the app

1. Open the deployed site.
2. Click **Upload schedule image**.
3. Choose a JPG, PNG, or WEBP schedule screenshot.
4. Wait for the OCR engine to load and finish parsing.
5. Review the day cards, shift table, and raw OCR text.

## Best results

- Crop tightly around the schedule grid.
- Use high-contrast screenshots when possible.
- If text is faint, increase the contrast slider.
- If parsing misses shifts, compare against the raw OCR text panel.

## Troubleshooting

### The page loads but OCR does not start

- Hard refresh the page.
- Make sure the browser allows JS from CDNs.
- Test the **Load working demo** button first.

Tesseract.js supports browser-based OCR projects, which is why this app can run on a static site without a backend.[cite:4]

### The OCR result is messy

- Crop the image tighter.
- Increase contrast.
- Use screenshots instead of camera photos when possible.
- Try uploading a cleaner weekly schedule image.

### GitHub Pages deploys but I see the wrong page

Make sure the app file is named `index.html` in the repository root, because GitHub Pages serves static content from the configured branch and folder.[cite:3]

## Updating the app

Replace `index.html` with a newer version, commit, and push again. GitHub Pages will republish the updated static site automatically after the new commit is processed.[cite:3]
