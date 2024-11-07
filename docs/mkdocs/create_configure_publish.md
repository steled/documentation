# Create, Configure, Publish

- how to create, configure and publish Material for MkDocs with GitHub Actions

## Source

- [MkDocs - Creating your site](https://squidfunk.github.io/mkdocs-material/creating-your-site/#creating-your-site)
- [MkDocs - Configuration](https://squidfunk.github.io/mkdocs-material/creating-your-site/#configuration)
- [MkDocs - GitHub Pages](https://squidfunk.github.io/mkdocs-material/publishing-your-site/#github-pages)

## Creating the site

- create a new GitHub repository
- create the following structure:

``` { .sh .no-copy }
.
├─ docs/
│  └─ index.md
└─ mkdocs.yml
```

## Configuring the site

- add at least the following lines to `mkdocs.yml`:

``` { .yaml }
site_name: My site
site_url: https://<username>.github.io/<repository>
theme:
  name: material
```

## Publishing the site

- add the following code to the file `.github/workflows/ci.yml`

``` { .yaml }
name: ci 
on:
  push:
    branches:
      - main
permissions:
  contents: write
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Configure Git Credentials
        run: |
          git config user.name github-actions[bot]
          git config user.email 41898282+github-actions[bot]@users.noreply.github.com
      - uses: actions/setup-python@v5
        with:
          python-version: 3.x
      - run: echo "cache_id=$(date --utc '+%V')" >> $GITHUB_ENV 
      - uses: actions/cache@v4
        with:
          key: mkdocs-material-${{ env.cache_id }}
          path: .cache
          restore-keys: |
            mkdocs-material-
      - run: pip install -r requirements.txt
      - run: mkdocs gh-deploy --force
```

- ensure that the [publishing source branch](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site) for your GitHub Page is set to `gh-pages`
- open your documentation at `<username>.github.io/<repository>`
