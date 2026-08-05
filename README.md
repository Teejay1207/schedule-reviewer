
# ShiftScope GitHub Pages

Static schedule OCR reviewer for weekly staff schedules, packaged so the OCR worker and language files live inside the repo.

## Important fix

The earlier build loaded Tesseract.js from a CDN and relied on default worker resolution. This package now points the app at local files inside `vendor/tesseract/`, so GitHub Pages can serve the script, worker, and language data from the same site origin.[cite:4][cite:22][cite:30]

## Included files

- `index.html` — main app.
- `vendor/tesseract/tesseract.min.js` — browser bundle.
- `vendor/tesseract/worker.min.js` — OCR worker.
- `vendor/tesseract/tesseract-core-simd.wasm.js` — wasm loader path used by the app.
- `vendor/tesseract/lang-data/eng.traineddata.gz` — English language data.
- `README.md` — setup notes.
- `.gitignore` — basic ignore file.

## Repo structure

```text
shiftscope-github-pages/
├── index.html
├── README.md
├── .gitignore
└── vendor/
    └── tesseract/
        ├── tesseract.min.js
        ├── worker.min.js
        ├── tesseract-core-simd.wasm.js
        └── lang-data/
            └── eng.traineddata.gz
```

## Deploy

1. Put all files in the repository root.
2. Commit and push.
3. In GitHub, open **Settings > Pages**.
4. Choose **Deploy from a branch**.
5. Select the main branch and `/ (root)`.
6. Save and wait for publish.

GitHub Pages serves static site files, which is why this package keeps the OCR assets in the repository instead of requiring a backend.[cite:3]

## Verify the files after deploy

Open these URLs in your deployed site and make sure they load:

- `/vendor/tesseract/tesseract.min.js`
- `/vendor/tesseract/worker.min.js`
- `/vendor/tesseract/tesseract-core-simd.wasm.js`
- `/vendor/tesseract/lang-data/eng.traineddata.gz`

If one of those returns 404, OCR will fail because the worker or language files are missing.[cite:22][cite:30][cite:34]

## Why you could not find Tesseract.js

In the previous package, Tesseract.js was referenced from a CDN script tag rather than stored as a file inside the repository. The Tesseract distribution includes separate browser bundle and worker files, which is why simply having one script reference is not always enough for a predictable static deployment.[cite:22][cite:26]

## Best results

- Upload tightly cropped screenshots.
- Prefer screenshots over angled camera photos.
- Increase contrast if the printed schedule is faint.
- Use the raw OCR text panel to inspect what the OCR engine actually read.
