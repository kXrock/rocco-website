# Rocco website

Public pages for the Rocco iPhone app, served by GitHub Pages from the public repo `kXrock/rocco-website`:

| page | address |
|---|---|
| Home | https://kxrock.github.io/rocco-website/ |
| Privacy Policy | https://kxrock.github.io/rocco-website/privacy.html |
| Support | https://kxrock.github.io/rocco-website/support.html |

Terms of Use: Apple's standard EULA (https://www.apple.com/legal/internet-services/itunes/dev/stdeula/).

The source lives in the private app repo (`kXrock/rocco`, folder `site/`). Edit it there, then publish:

```bash
bash site/publish.sh "Privacy: mention X"
```

The same addresses are used in the app (`mobile/src/shared/config/links.ts`), in App Store Connect
(Privacy Policy URL, Support URL, Marketing URL) and in Google Auth Platform (Branding).
When the privacy policy changes, update its "Effective" date and the App Privacy answers in App Store Connect.
