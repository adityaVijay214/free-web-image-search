# Free Web Image Search V3

**Find educational and reference images from the open web and insert them into Obsidian as remote URLs.**

> **The plugin inserts remote image URLs and does not download selected images into your Obsidian vault.**

## Installation

Extract `Free-Web-Image-Search.zip` to:

```text
YOUR_VAULT/.obsidian/plugins/free-web-image-search/
```

Enable **Free Web Image Search** under **Settings → Community plugins**. The base plugin requires no user API key, credit card, billing account, mandatory login, AI service, telemetry, or subscription.

## V3 capabilities

V3 retains the V2 deterministic search pipeline, provider failure isolation, pagination, relevance ranking, deduplication, context search, preview, favorites, history, and remote-image health checks. It adds a larger normalized source ecosystem, source-aware provider selection, rights-aware filtering, a reusable IIIF URL layer, and specialist chemistry and structural-biology providers.

Only the explicit query is sent to a provider. Note contents, paragraphs, vault files, analytics, and telemetry are not sent anywhere.

## First-class public providers

| Provider | Access status | Direct image capability | Rights / metadata notes |
|---|---|---|---|
| Wikimedia Commons | Public, no user key | Original file URL and thumbnail | ImageInfo author, license, description, categories, dimensions, SVG support |
| Openverse | Public, no user key | Returned image URL and thumbnail | Creator, license, category, dimensions where returned |
| Wikipedia | Public REST search | Article original or thumbnail image | Separate article-image provider; rights must be checked at the source |
| Library of Congress | Public API | Image URLs where returned | Historical metadata, subjects, collections, creator, date, rights fields |
| Internet Archive | Public Advanced Search | Archive image-service URL for image items | Only image-media results are normalized; arbitrary books/PDFs are not presented as ordinary images |
| The Met | Public Collection API | Primary image URL | Open Access/public-domain status is preserved; not all objects have identical rights |
| NASA | Public Images API | Official preview/image asset | NASA source metadata and rights caveat are shown |
| Art Institute of Chicago | Public API and IIIF | Official IIIF image URL | Public-domain flag from API; IIIF info URL included |
| Cleveland Museum of Art | Open Access API | Full/web image URL where supplied | Share-license/rights state is preserved; missing images are skipped |
| SMK | Public collection API | Native or thumbnail image URL | Rights fields are shown only when supplied |
| PubChem | Public PUG REST | Official remote PNG structure image | Specialist chemistry provider; formula and compound page are included |
| RCSB Protein Data Bank | Public search API | Official structure image CDN URL | Specialist macromolecule provider; structure page and metadata are included |

The following sources are not silently scraped or falsely represented as working: Gallica/BnF, Wellcome, Rijksmuseum, Getty, National Gallery of Art, Walters, USGS, and NOAA. Their current no-key image endpoints and direct-image semantics require additional endpoint-specific verification before being enabled. Credentialed sources such as Smithsonian, Europeana, Harvard, DPLA, NYPL, Flickr, Pexels, Unsplash, and BHL are not required by the base plugin.

## IIIF support

The reusable `buildIIIFUrl()` helper supports provider-supplied IIIF services with full images, width-based resizing, regions, rotation, quality, and format selection. The Art Institute provider uses its official IIIF service and exposes `info.json`. The URL is built from provider image identifiers, not from arbitrary scraped HTML.

## Search and source-aware modes

Search modes include General, Educational, Diagram, Scientific, Mathematics, Physics, Chemistry, Biology, Geography, History, Maps, Architecture, Engineering, Anatomy, Astronomy, and Military History. Queries are expanded deterministically; no AI API is involved.

The source picker includes individual V3 sources. Specialist searches work naturally with `glucose`, `benzene`, `aspirin`, `hemoglobin`, `Saturn`, maps, diagrams, and reference imagery. Context commands search selected text, current heading, note title, or current paragraph without uploading surrounding content.

## Rights and access-cost distinction

**Free / no-user-key** means the API can be used without user credentials. It does **not** mean every returned image is copyright-free. Cards and previews distinguish returned image-rights information such as Public Domain, CC0, Creative Commons, Open Access, Rights information available, or Rights information unknown.

The **Open license only** filter keeps results only when the provider returned reliable rights/license/public-domain metadata. It does not infer rights from an institution name. Licenses and license URLs are never invented.

## Result actions and insertion

Result cards provide preview, insert, copy direct URL, copy Markdown, favorite, open source, context menu, and keyboard actions. Preview displays title, creator, date, collection, dimensions, format, license, rights status, source, direct URL, and IIIF information when returned.

Default insertion remains:

```markdown
![Alt text](https://example.org/direct-image-url.jpg)
```

Optional settings support alt-text prompts, captions, compact/full attribution, and open-license filtering. URLs are limited to `http://` and `https://`. The plugin never uses attachment upload, never writes image binaries to the vault, and never substitutes a local copy.

## Remote image health

**Manage Remote Images** and **Check Remote Images** scan Markdown remote-image links in the current note, current folder, or entire vault. Results include note, line, alt text, URL, HTTP status, and detected MIME when available. Checks use bounded concurrency and never replace links automatically.

## Provider health and limitations

Provider errors are isolated and shown alongside successful results. Public services can rate-limit, change endpoints, reject requests from a network edge, or block HEAD requests. Metadata differs by provider. A provider may return a source page and thumbnail but no stable original; only a validated returned URL is used. Some image hosts may block hotlinking or later remove assets.

In this build environment, the Library of Congress API may return HTTP 403 through the sandbox network edge even though the provider implementation uses its official public API. It is therefore isolated and reported as unavailable rather than hidden or misrepresented.

## Privacy and security

No note content is uploaded. Only explicit search queries are sent. There is no AI integration, analytics, telemetry, account requirement, or credential collection. Unsafe URL schemes such as `javascript:`, `data:`, `file:`, and `chrome:` are rejected. Provider metadata is rendered with safe text APIs.

## Development and verification

```bash
npm install
npm run build
```

V3 uses TypeScript as the source of truth and esbuild for the generated `main.js`. The final ZIP contains the compiled plugin, source, stylesheet, manifest, package metadata, TypeScript configuration, README, and data-shape reference file. No image files are included.

## Changelog

### 3.0.0

- Added Wikipedia, Art Institute of Chicago, Cleveland Museum of Art, SMK, PubChem, and RCSB PDB providers.
- Added normalized rights states, public-domain flags, IIIF metadata, and IIIF URL generation.
- Added source-aware no-key provider selection and open-license filtering.
- Added source-specific metadata details in cards and preview.
- Added specialist chemistry and structural-biology image search.
- Preserved remote-only insertion, deterministic ranking, pagination, health checks, favorites, history, and provider failure isolation.
