# Group template

A template for one **subject** (for example "Habit"): a public GitHub Pages
site that lists the subject's books. Everything book-derived is encrypted and
named so that nothing in this repo says what the subject or the books are.
The code that shows the hub, reads a book and runs the quiz is **not** here:
it comes from [`eckko/group-ui`](https://github.com/eckko/group-ui) (the hub)
and [`eckko/item-ui`](https://github.com/eckko/item-ui) (reader and quiz) on
jsDelivr.

```
<subject repo>/            public, Pages from main, folder /
  index.html               the hub loader: loads group-ui at the version in .grouprc
  .grouprc            one line: the hub UI version ("3" = newest 3.x.x)
  .itemrc             one line: the exact item-ui tag (written by publishing)
  boot.json                the sign-in theme only ({} until you add one)
  index.enc                encrypted index: title, books, settings (after the first publish)
  r.html q.html            reader and quiz pages, opened as r.html#b=<book id>
  .theme/                  sign-in pictures and styles (no book names)
  xx/<30 hex>.bin          the encrypted pool, flat, names say nothing
  .github/workflows/       check-site.yml, fork-warning.yml, auto-pr.yml
```

The format and rules are in item-ui's `ai-context/opaque-site.md`. The key is
**never** stored in any repo; one key (`SUBJECT_KEY`) opens every book of the
subject.

## Versions

`.grouprc` and `.itemrc` are one-line files, so a release can change
them without touching anything else:

| File | Holds | Who sets it |
|---|---|---|
| `.grouprc` | `3` (newest 3.x.x), `3.1` or `3.1.0` | you, or `update-rc.yml` after a group-ui release (a pull request that sets the major) |
| `.itemrc` | an exact tag such as `3.4.0` | publishing (`build_pool.py`), or `update-rc.yml` after an item-ui release (a pull request) |

The release workflows of item-ui and group-ui send the event (secret
`TEMPLATE_UPDATE_TOKEN` there); merging the pull request adopts the version.
Needs "Allow GitHub Actions to create and approve pull requests" in this repo's
Actions settings. `.nojekyll` must stay: without it GitHub Pages hides the two
dot files and the hub cannot start. Existing subject sites are moved by
`update-subject.yml` in `eckko/actions`.

## Start a new subject

1. **Use this template → Create a new repository** (public).
2. Settings → Pages → Deploy from a branch, `main`, `/ (root)`.
3. Add the secret `SUBJECT_KEY` and make the **check** workflow a required
   check for `main`.
4. Create a fine-grained token limited to this repo with **Contents: Read and
   write**; the book repos use it as `SUBJECT_REPO_TOKEN`.
5. Sign-in look (optional): add `.theme/` files and name them in `boot.json`
   (see group-ui's README, "Theme a subject").
6. Create a book with **Create book** in `eckko/actions`; its publish workflow
   writes `index.enc` and the pool here.

Hub, status and admin features (Add a book, Update a book) are in group-ui.
