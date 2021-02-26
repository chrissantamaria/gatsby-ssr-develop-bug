# Gatsby SSR in Develop Bug

This repo is a minimum reproduction of a bug with the [new `DEV_SSR` flag](https://github.com/gatsbyjs/gatsby/discussions/28138) in Gatsby where page contents aren't properly included in the initial load.

## Reproduction Steps

1. Clone the repo
2. Install dependencies:

```bash
yarn
```

3. Clean + start the dev server:

```bash
yarn clean && yarn develop
```

4. After a successful build (shows "You can now view gatsby-ssr-develop-bug in the browser."), fetch `http://localhost:8000`. This can be done in a browser but for the sake of demo we can use `curl`:

```bash
$ curl --silent http://localhost:8000 | grep --count '<h1>Home</h1>'
0
```

Observe the lack of page content (`<h1>Home</h1>`) in the response.

5. Repeat the above step and observe it now having the correct page contents:

```bash
$ curl --silent http://localhost:8000 | grep --count '<h1>Home</h1>'
1
```

A more explicit error is shown if performing the initial load in a browser and opening the console:

```
index.js:2177 Warning: Expected server HTML to contain a matching <h1> in <div>.
    in h1 (at pages/index.jsx:3)
    in IndexPage (created by HotExportedIndexPage)
    in AppContainer (created by HotExportedIndexPage)
    in HotExportedIndexPage (created by PageRenderer)
    in PageRenderer (at query-result-store.js:90)
    in PageQueryStore (at root.js:57)
    in RouteHandler (at root.js:79)
    in div (created by FocusHandlerImpl)
    in FocusHandlerImpl (created by Context.Consumer)
    in FocusHandler (created by RouterImpl)
    in RouterImpl (created by Context.Consumer)
    in Location (created by Context.Consumer)
    in Router (at root.js:74)
    in ScrollHandler (at root.js:70)
    in RouteUpdates (at root.js:69)
    in EnsureResources (at root.js:67)
    in LocationHandler (at root.js:125)
    in LocationProvider (created by Context.Consumer)
    in Location (at root.js:124)
    in Root (at root.js:133)
    in StaticQueryStore (at root.js:149)
    in ConditionalFastRefreshOverlay (at root.js:148)
    in _default (at app.js:166)
```

## Workarounds

Changing `src/pages/index.jsx` to use a `.js` extension fixes this issue.
