# okf-bundle-template

A GitHub template for publishing a website as a self-updating
[OKF](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md)
bundle you can add to the
[awesome-okf-kit](https://github.com/vinodborole/awesome-okf-kit) registry.

## Use it

1. **Create a repo from this template** (green “Use this template” button).
2. **Run the _Build bundle_ action** (Actions tab → *Build bundle* → *Run
   workflow*) with your site URL and a bundle name. It crawls the site,
   commits the bundle, and publishes a `v0.1.0` release zip.
3. **Edit `NOTICE.md`** with the source's license/attribution.
4. **Add it to the registry**: open a PR to
   [awesome-okf-kit](https://github.com/vinodborole/awesome-okf-kit) adding a
   `registry.yaml` entry pointing at your release zip
   (`…/releases/latest/download/<name>-okf.zip`).

The included **weekly sync** action keeps the bundle fresh — `okf sync` only
rewrites changed pages, so commits stay small.

> Publish only content you may redistribute — your own site, or permissively
> licensed content (CC-BY, CC-BY-SA, MIT/Apache project docs, public domain).

Built with [okf-kit](https://github.com/vinodborole/okf-kit).
