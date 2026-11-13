# C2-SYM synthetic Airtable Blocks template fixture

Purpose: a **controlled** test of whether `block init --template <GitHub-URL>`
may copy contents of a symlink target *outside the template* into a new project.
Use only on a machine you own/control with a synthetic, non-sensitive marker.

## Files

- `block.json`: standard frontend entry (`./frontend/index.js`).
- `package.json`: minimal Airtable/React dependencies; **no install hooks**.
- `frontend/index.js`: a minimal extension that **does not import the marker**.
- `frontend/diagnostic-marker.txt`: a real Git symlink; target is exactly
  `/tmp/airtable-c2sym-marker.txt` on the computer importing the template.

The archive does **not** contain the target file or any credentials. The target
must be created locally on the test Mac before a genuine CLI import:

```sh
printf 'C2SYM_SYNTHETIC_ONLY_20261114\n' > /tmp/airtable-c2sym-marker.txt
```

## Important when uploading to GitHub

The symlink must remain a **symlink** (Git mode `120000`). Do not upload the ZIP
file as a single GitHub file or drag its extracted symlink through GitHub's web
file uploader; the web uploader is not a reliable way to preserve symlink type.
Instead, extract on the Mac, then add, commit and push the extracted directory
using Git. Check it before pushing with:

```sh
readlink frontend/diagnostic-marker.txt
# Expected: /tmp/airtable-c2sym-marker.txt

git ls-files --stage frontend/diagnostic-marker.txt
# After git add: expected mode 120000
```

## Test goals (to run later, step-by-step)

1. Push this template as a researcher-owned GitHub repository, preserving symlink.
2. Confirm synthetic marker exists on the importing Mac.
3. Use genuine `block init APP_ID/BLOCK_ID PROJECT_DIR --template GITHUB_URL`.
4. Inspect the created project **locally** and record whether the symlink remains
   a link, becomes a copied regular file, or is rejected.

**No malicious package scripts, no uploads, no telemetry, no exfiltration** are
included here. Do not place any real API keys, PATs, SSH keys, or private files
at the target path. Do not run `block release` or `block submit` for this test.

This fixture alone does not establish that a security vulnerability exists.
