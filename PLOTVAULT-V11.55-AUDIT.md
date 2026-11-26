# PlotVault v11.55 — Code Audit Notes

## Scope

Static code review and focused logic checks of `index.html` on the `plotvault-code-audit` branch of `andydace/buildproof-preview`.

This is an audit candidate, not a declaration that the app is production-ready or free of defects.

## Corrections made

- Made photo capture wait for image decoding and resizing before opening the category screen; added image type/size checks and failure handling.
- Kept GPS optional so a denied or unavailable location does not prevent saving evidence; preserved valid zero-coordinate values.
- Made multi-key localStorage saves verify writes and attempt rollback after failure.
- Made data loading validate records before committing them to in-memory state.
- Added duplicate checks and cancellation-safe editing for projects and plots.
- Made project and plot deletion restore memory after a failed save.
- Made evidence approval/rejection and deletion transactional.
- Kept checklist state consistent when multiple photos exist for one evidence category.
- Recalculated compliance when a new photo moves an item back to pending review.
- Hardened dynamic event arguments against apostrophe-based attribute injection.
- Added defensive handling for missing screens, controls, plots and checklist data.
- Sanitised report HTML before opening, downloading or sharing it; corrected missing-GPS handling in report registers.
- Corrected report labels so the app does not claim it has generated a PDF when it currently creates an HTML report and opens the browser print dialogue.
- Removed duplicate static logo IDs and leftover debug logging.

## Checks completed

- JavaScript syntax compilation: PASS.
- Duplicate function declarations: none found.
- Duplicate IDs in static page markup: none found.
- Inline event-handler references: all recognised.
- Leftover console debug logging: none found.
- Focused logic checks: approval, rejection with multiple photos, missing-photo approval prevention, failed-save rollback, evidence deletion status, plot duplicate prevention, and localStorage save rollback passed.

## Important limitations before commercial release

1. **Data storage:** evidence images are stored as base64 data inside browser localStorage. Browser storage limits are small and vary by device/browser. A development with many plots and photographs will outgrow this approach. Move evidence files to IndexedDB and/or secure cloud object storage before commercial deployment.
2. **Accounts and security:** the current sign-in/create-account screen is a prototype. There is no real user authentication, account recovery, permissions or secure multi-user access.
3. **Cloud and multi-device:** records are stored locally in the browser. There is no cloud database or cross-device synchronisation. Remote notifications are therefore not implemented.
4. **PDF and email:** reports are HTML documents opened in the browser print flow. Users must choose Save as PDF. The share function shares/downloads an HTML report; it does not automatically attach a PDF to an Outlook email. This requires a PDF generation and email integration service.
5. **Runtime testing:** the code checks above do not replace testing in Safari on the target iPhone, desktop browsers, camera/GPS permissions, printing, sharing, backups and realistic photo volumes.

## Visual design

The legacy CSS contains numerous successive override blocks. They have been left in place during the functional audit to avoid introducing layout regressions. A separate, controlled visual redesign should consolidate the styling and introduce the requested richer, more vibrant and polished finish after the v11.55 functional preview is checked.

## Release status

**Audit candidate for preview only.** Do not use this branch as a commercial production release until the storage, authentication, cloud, reporting and device-testing requirements above have been addressed.
