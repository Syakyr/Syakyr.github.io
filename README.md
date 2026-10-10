# Syakyr.github.io

User-site root for `syakyr.github.io`. Exists mainly so the **origin root** is
ours, which is where Digital Asset Links must be served from.

```
https://syakyr.github.io/.well-known/assetlinks.json
```

That file delegates the domain to the Android Trusted Web Activity that wraps
<https://syakyr.github.io/sudoku/>. Without it the APK installs and runs, but
Chrome falls back to a Custom Tab with a URL bar instead of a trusted
full-screen app.

- `.nojekyll` — disables Jekyll processing so dotfiles under `.well-known/`
  are served verbatim instead of being skipped or mangled.
- `.well-known/assetlinks.json` — the delegation. `sha256_cert_fingerprints`
  is an **array on purpose**: add a fingerprint for every signing key you ever
  use, rather than replacing it. Rotating a single entry breaks verification
  for builds signed with the old key.

## If verification stops working

1. `curl -i https://syakyr.github.io/.well-known/assetlinks.json` → must be
   `200` and `application/json`.
2. The fingerprint must match the key that actually signed the installed APK:
   `keytool -list -rfc -keystore <keystore> | openssl x509 -noout -fingerprint -sha256`
3. `package_name` must match the applicationId baked into the APK.

Both values are printed by the `Android TWA release` workflow in the sudoku
repo.
