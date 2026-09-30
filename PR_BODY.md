## Summary

The [2026-09-30 estate composition audit](https://github.com/isaackoi/Phoenix-Alpha-workbench/issues/4439#issuecomment-5915537552) found every page ships a per-anchor `data-sidebar-search="..."` attribute on all ~730 sidebar anchors while the entire JS bundle reads it in exactly ONE place — the cosmetic `cleanGeneratedTopicLabels` loop, which skips absent attributes. Sidebar search is served by the lazy `assets/js/search-index.js` asset plus the title/textContent fallback harvester (`collectSiteSearchFallbackPages` reads `title` + `textContent` + `data-card-search`, never `data-sidebar-search`). The attribute is dead payload (~30 KB decoded on EVERY page of the site).

- **_includes/sidebar.html**: stops emitting the attribute (2 emission site(s) removed; `title` tooltips and `aria-label` a11y enrichment untouched).
- **assets JS**: the cleanup list drops the dead attribute name (title/aria-label cleanup unchanged).
- **_config.yml**: `ui_bundle_version` bumped for cache-busting.

Zero editorial content touched; search behaviour unchanged (index asset path); zero deploy actions in the producing session.

Part of the estate page-weight trim work item (alpha-production:estate-sidebar-search-index-bundle:v1, #4439).