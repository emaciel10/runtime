# Temporary PDFium v2 maintenance branch

This branch is pinned to EmbedPDF's native revision
`73a041a5c1aaffa09ebb64f1981ff501cb91a41c`, used by the released
`@embedpdf/pdfium` 2.15.1 package. It adds the gradient pattern stroke paint-order
fix and an independent synthetic regression fixture/test. The upstream submission
is https://github.com/embedpdf/runtime/pull/76.

The initial `2.15.1-hive.1` package retains the official 2.15.1 JavaScript
wrapper and replaces its packaged WASM with the previously rebuilt and verified
v2 payload. This packaging step does not rebuild PDFium. The WASM SHA-256 is
`2b97c7f489d6577feea8bf2d6319bf81e064994f5e92bdec1a072dcf837cdf14`.
Its 37 imports and 630 exports match the original payload.

GitHub releases hold the npm tarball and its checksums. Consumers can pin the
tarball URL plus their lockfile integrity instead of committing a WASM file.
The tarball contains `FORK_PROVENANCE.json` and `FORK_BUILD.md`.
Customer PDFs, screenshots and copied records are not part of the package.

## Rebuild

Use the EmbedPDF viewer/build sources at
`dfb2df818065c704a0241beb0791e2fd0d55a10e`, its PDFium build-system patches,
and Emscripten 3.1.70. Replace its native submodule with this branch's pinned
commit. Keep the JavaScript wrapper on 2.15.1.

The upstream native unit/embedder suite and AGG/Skia rendering checks remain
required for a fresh native build. A prior binary's passing checks do not certify
a new binary. After building, verify import/export parity, run
`FPDFRenderPatternEmbedderTest.ShadingStrokePaintsAfterSolidFill`, and rerender
the affected document at 1, 2 and 4 pixels per point. Publish a new immutable
version and checksum when any source, toolchain or payload changes.

## Exit

Return to a maintained upstream v2 package after PR #76 or an equivalent fix is
released and passes the same regression and customer-file checks. This branch
does not switch Hive dependencies, merge a Hive PR or deploy an application.
