# Desktop Golfer 2D Screenshots

The screenshots of the store pages of Desktop Golfer 2D, kept apart from
the game: a JPEG of 2880x1800 for each screenshot in each language, made
anew for a version, would weigh down the history of the game.

```
store/appstore/
  en-US/
    desktop/
      screenshot_01.jpg     # in the order of the numbers
      ...
  ja/
    ...
```

They are captured from the game by its tools for them, not edited by hand.

The game keeps none of them, but a list of them, which the store takes
them by:

```sh
rake yml              # store/appstore/<language>/screenshots.yml here
rake yml root=DIR     # in DIR/store/appstore, as the game
```

```yaml
- path: desktop/screenshot_01.jpg
  url: https://raw.githubusercontent.com/<this repository>/<HEAD>/store/appstore/ja/desktop/screenshot_01.jpg
  md5: 3b2454a4d9d4298ed252ca9efbbdc7e9
```

- By the URLs of HEAD, which do not change: committed and pushed first,
  or it writes none.
- In the order of the numbers; the list of a language with no screenshot
  here is deleted.
- The lists written here are left out of the repository.
