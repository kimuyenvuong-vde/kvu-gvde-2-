# Thank you page: Giamo & Kim Uyên, 29 August 2026

The post-wedding companion to the invite site. Same design system: Cinzel, Cormorant Garamond and
EB Garamond, the gold and lacquer tokens, light/dark toggle, EN/NL/IT/VI, falling petals, music
button. Static HTML, no build step.

```
index.html                  the whole page (markup, styles, script)
assets/
  *-thumb.jpg               shown in the carousel
  *.jpg                     loaded full screen in the zoom viewer
  monogram-gold.png         used in dark theme
  monogram-dark.png         used in light theme
music.mp3                   background track, plays via the top bar button
.nojekyll                   tells GitHub Pages to serve files as they are
```

## The music

`music.mp3` sits next to `index.html` and is wired up already. It is the track you supplied,
re-encoded to 128 kbps mono-friendly stereo (2.9 MB instead of 4.3 MB) with the ID3 tags stripped,
so it loads fast on a phone.

Behaviour: `preload="none"` so nothing downloads until it plays, it tries to autoplay and otherwise
starts on the first click, keypress or tap, it fades in over about a second and a half to 55 percent
volume rather than opening at full blast, it pauses when the tab is hidden, and the ♪ button toggles it.

To swap the track later, drop in a new file and change one line:

```html
<source src="./music.mp3" type="audio/mpeg">
```

To change how loud it starts, edit `TARGET_VOLUME` in the music block near the bottom of the file.
To remove music entirely, delete the `<audio>` element, the `.music-btn` button in the top bar, and
the music block in the script.

## Publish on GitHub Pages

1. Put these files in a repo, or in a `thanks/` folder inside `kvu-gvde`.
2. Settings, then Pages, then Deploy from a branch, `main`, folder `/ (root)`.
3. A repo of its own lands on `https://<user>.github.io/<repo>/`. A subfolder lands on
   `https://kimuyenvuong-vde.github.io/kvu-gvde/thanks/`.

## Changing things

- **Text**: everything lives in the `T` object near the bottom of `index.html`, one block per
  language (`en`, `nl`, `it`, `vi`), same shape as the invite site. The letter is built from
  `letter_p1`, `letter_p2` and `letter_p3`.
- **Photos**: the `PHOTOS` array sets the carousel order. Each entry needs `assets/<name>.jpg`,
  `assets/<name>-thumb.jpg`, and a caption key in all four language blocks.
- **When the photographer delivers**: add the files, add entries to `PHOTOS`, then replace the
  `pending` line in each language block with whatever you want to say instead.
- **Links**: the two `<a class="btn-link">` tags hold the photo booth and upload URLs.

New photos, to match what is already in `assets/`:

```bash
magick input.jpg -resize 1600x -quality 84 assets/name.jpg
magick input.jpg -resize 640x  -quality 78 assets/name-thumb.jpg
```

## Notes

- Light theme is the default here, always. The page ignores the system colour scheme and the
  invite site's saved theme, and only goes dark if the visitor presses ☽ on this page. That choice
  is kept under its own key, `thanksTheme`, so it never fights the invite.
- Language is still shared with the invite via `weddingLang`, so a guest who picked Vietnamese
  there gets Vietnamese here.
- Keyboard: arrows move through the carousel and viewer, `+` and `-` zoom, Escape closes.
- Honours `prefers-reduced-motion`: petals and the scroll line stop.
