
The carousel is in. The full cycle is now: Names flip in → Hold 2 min → Company logo carousel → Date/time → Restart.
Everything you need to configure lives at the top of the <script> block. The key pieces:
ENABLE_CAROUSEL — set to false to skip it entirely for days when you don't need logos.
CAROUSEL_SLIDE_TIME — how long each company stays on screen (default 5 seconds).
COMPANIES array — each entry takes a name and a logo_src. Right now it has placeholder names with empty logo_src values, which renders the company name as text inside the logo frame. When you're ready with real logos, you have two options: point logo_src to a file path like "logos/acme.png" (relative to the HTML file), or embed as base64 with "data:image/png;base64,...". The carousel shows each company one at a time with a fade-in animation and progress dots at the bottom.
