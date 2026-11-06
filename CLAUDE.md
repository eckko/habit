This repo is the shell for one subject site: a public GitHub Pages site whose
books are all encrypted. It holds `index.html` (a fixed loader), `boot.json`
(the sign-in theme only), `.grouprc`, `.itemrc`, `index.enc`, `r.html`,
`q.html`, `.theme/`, a flat pool of `xx/<30 hex>.bin` files and the workflows.
It must never name the subject or a book anywhere readable (page, file name,
workflow, commit message). Never add a plain book file, image or key, never
edit the pool by hand, never change a book's id.
No UI code lives here. `index.html` loads the hub from
https://github.com/eckko/group-ui at the version in `.grouprc` (`3` = newest
3.x.x); the reader and quiz pages load https://github.com/eckko/item-ui at the
exact tag in `.itemrc`. `build_pool.py` (item-ui, `tools/site/`) writes
`.itemrc`, `index.enc`, `r.html`, `q.html` and the pool when a private
book repo (made from item-template) publishes. The release actions of group-ui
and item-ui may update the two version files. See README.md
