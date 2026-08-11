# elvinhu.com

A two-page static site. No build step, no dependencies — plain HTML and CSS.

```
index.html            Home
ito/index.html        Project Ito
css/style.css         Shared base
css/project.css       Project-page layer
assets/               Images and video
```

## Run locally

```bash
python3 -m http.server 8000
```

Then open <http://localhost:8000>.

One caveat: `http.server` ignores `Range` requests, so seeking in the walkthrough
video snaps back to the start. That is the server, not the file — any real static
host handles it. Use a range-aware server if you need to check playback.

## Deploy

Any static host works. For GitHub Pages, push to a repository and enable Pages on
the branch root; `ito/index.html` is served at `/ito/`.

For a custom domain, add a `CNAME` file at the repo root containing the bare
domain, then point DNS at GitHub: four `A` records for the apex
(185.199.108–111.153) or a `CNAME` to `<user>.github.io` for a `www` subdomain.

## Assets

Thumbnails and video are referenced by path and fall back to a grey block when the
file is missing, so the layout holds either way.

Entry thumbnails are squares, 512×512. The professional and side-project ones come
from the lead carousel slide of each project on elvinhu.com, which are all 1024×498
banners on a flat background colour. To square them without losing the composition,
each subject is cropped to its content box, scaled to 88% of the square, and padded
out with the banner's own background colour — so the padding is invisible.

| Path | Notes |
| --- | --- |
| `assets/thumbs/genai-chrome.jpg` | From elvinhu.com |
| `assets/thumbs/material-you.jpg` | From elvinhu.com. A photo, so cropped square around the left phone rather than padded |
| `assets/thumbs/chrome-brand.jpg` | From elvinhu.com |
| `assets/thumbs/form-factors.jpg` | From elvinhu.com |
| `assets/thumbs/ios-widgets.jpg` | From elvinhu.com |
| `assets/thumbs/album-wheel.jpg` | Frame from the site's video, padded on black |
| `assets/thumbs/ito.jpg` | Poster frame behind the animated Ito thumbnail |
| `assets/avatar.jpg` | 400×400, cropped to head and shoulders so the face still reads at 56px |
| `assets/elvin-hu-cv.pdf` | Public build. Keep the CHI submission out of it while that paper is under review |
| `assets/video/ito-thumb.webm` | Animated home-page thumbnail for Ito |
| `assets/video/walkthrough.mp4` | x264 CRF 21, 27 MB, from `Ito Final.mov` |
| `assets/video/walkthrough-poster.jpg` | Poster frame for the above |
| `assets/video/lifecycle-*.mp4` | Square silent loops, one per phase |

To regenerate one, crop to the content box, scale, then pad with the background
colour (`--cropOffset` takes y then x):

```bash
sips -c <h> <w> --cropOffset <y> <x> source.png --out /tmp/a.png
sips -Z 450 /tmp/a.png --out /tmp/b.png
sips -p 512 512 --padColor AECBFA /tmp/b.png --out /tmp/c.png
sips -s format jpeg -s formatOptions 84 /tmp/c.png --out assets/thumbs/name.jpg
```

Recordings from the extension are in `assets/video/source/`. They are 16:9, so
crop to square before using them as lifecycle clips:

```bash
ffmpeg -i assets/video/source/2_Create_a_Flow.mp4 \
  -vf "crop=ih:ih" -an -movflags +faststart \
  assets/video/lifecycle-creation.mp4
```

## Placeholders to replace

- The arXiv and GitHub buttons on the Ito page, currently marked "coming soon".
