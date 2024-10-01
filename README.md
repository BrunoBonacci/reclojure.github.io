# reClojure

This repo contains the website for https://reclojure.org

## Development

CLI commands listed below should be run from the project’s root.

Also, make sure you have done `npm install` at least once.

### Emacs and Cider setup

Run `cider-jack-in-clj&cljs` and then eval `(go)` at the REPL to start
the development http server at http://localhost:7070

### Other environments

    clj -M:dev:cider-nrepl
    (go)
    (cljs-repl)

Open http://localhost:7070

## Build

To build the project, run the following command:

```
bb build
```

This command creates and populates the `docs` directory with the
result of running the build process merged with the files that are in
`resources/public`. `docs` is the directory that should be published.

## Local server to inspect build results

To test the output of the build process locally before deploying, run
a local dev server with `bb server` and visit `localhost:8080`.

Note that this currently uses Python 3, but from within `docs`, any
other local web server will probably do.

## Deploy

Merge your commits into master and it will be published live automatically by
gh-action.

This is how the deploy works:
- github action gets triggered
- it calls the `-X:freeze` command which use reitit-jaatya to build the latest
  site - atm it's the 2022 version
- it creates a static site out of the sessionize and `db.clj` data and spits it
  out into `_site`
- then it copies the the `2021` folder in root to `_site`. This folder is a
  "BUILT" snapshot, we shouldn't edit this anymore
- finally gh-actions will copy all the contents of `_site` to a branch
  `gh-pages`
- this branch is what gets displayed on reclojure.org

## Cleanup

After the build is created and deployed, make sure to run `bb clean`
if you branch from master in order to resume development, otherwise
the CSS generated by the build process will stop newer CSS from
loading.
