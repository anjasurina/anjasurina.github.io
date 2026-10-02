---
layout: default
title: CV
permalink: /cv/
description:
---

{% assign cv_url = '/assets/pdf/cv.pdf' | relative_url | bust_file_cache %}

<div class="cv-frame" id="cv-frame" data-src="{{ cv_url }}">
  <iframe src="{{ cv_url }}#view=FitH&navpanes=0" title="Anja Šurina CV"></iframe>
</div>

<script type="module">
  // Phones and tablets often cannot show PDFs inside a page, so draw it with PDF.js there.
  const frame = document.getElementById("cv-frame");
  const touch = window.matchMedia("(pointer: coarse)").matches;
  if (touch || !navigator.pdfViewerEnabled) {
    const pdfjs = await import("https://cdn.jsdelivr.net/npm/pdfjs-dist@4.10.38/build/pdf.min.mjs");
    pdfjs.GlobalWorkerOptions.workerSrc = "https://cdn.jsdelivr.net/npm/pdfjs-dist@4.10.38/build/pdf.worker.min.mjs";
    const pdf = await pdfjs.getDocument(frame.dataset.src).promise;
    const pages = document.createElement("div");
    pages.className = "cv-pages";
    const width = frame.clientWidth;
    const ratio = window.devicePixelRatio || 1;
    for (let n = 1; n <= pdf.numPages; n++) {
      const page = await pdf.getPage(n);
      const viewport = page.getViewport({ scale: (width / page.getViewport({ scale: 1 }).width) * ratio });
      const canvas = document.createElement("canvas");
      canvas.width = viewport.width;
      canvas.height = viewport.height;
      await page.render({ canvasContext: canvas.getContext("2d"), viewport }).promise;
      pages.appendChild(canvas);
    }
    const link = document.createElement("a");
    link.href = frame.dataset.src;
    link.className = "cv-open";
    link.textContent = "open pdf";
    frame.replaceChildren(pages, link);
  }
</script>
