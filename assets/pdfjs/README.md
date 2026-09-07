# PDF.js report viewer

The posts use the self-hosted **PDF.js 6.3.289 legacy build** from Mozilla:
https://github.com/mozilla/pdf.js/releases/tag/v6.3.289

Source archive: `pdfjs-6.3.289-legacy-dist.zip`. Source maps and the sample PDF are
omitted. Upstream licenses are included. The legacy build provides the
compatibility code needed by Safari.

The Pages workflow downloads this archive, verifies its SHA-256 checksum, and
extracts it into `assets/pdfjs/6.3.289/` before Jekyll builds the site. This generated
directory is ignored by Git. The workflow also checks that Jekyll copies the
complete viewer distribution into the site's deployment directory under `_site`.

For local previews, run the commands from the workflow's `Download PDF.js 6.3.289`
step before starting Jekyll, with `PDFJS_VERSION=6.3.289` and `RUNNER_TEMP` set to a
temporary directory.

The only upstream modification is `data-proofer-ignore` on the signature image
browse link in `web/viewer.html`: PDF.js fills this control at runtime, so it has
no static `href` for the site's HTML checker to validate.

`_includes/pdf-report.html` passes a same-origin PDF URL and opens the viewer at
`page-width` with the sidebar closed. The HTML viewer renders the document itself,
avoiding mobile browsers' native PDF iframe sizing. It also handles resizing,
page navigation, zoom, text selection, search, and downloads. The direct PDF link
remains available when JavaScript is disabled or the viewer cannot load.

To embed another report in a post:

```liquid
{% include pdf-report.html file="/assets/CyberOps/report.pdf" title="Investigation report" %}
```

When updating PDF.js, update the workflow's version, archive checksum, and output
verification paths, the ignored version directory in `.gitignore`, and
`viewer_url` in the include. Keep using a complete legacy release (excluding source
maps and the sample PDF) so the viewer, library, worker, and supporting assets stay
on the same version. Retain the workflow's HTML checker adjustment if still needed.
Check every report at phone and desktop widths after an update.
