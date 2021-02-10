### Basics

+ Semantic html: `<nav> <header> <footer>` over `<div> <p>`

  + content as keywords for search engine
  + guide screen readers
  + easier maintenance

+ Accessibility:

  + semantic
  + keyboard accessibility: tabindex
  + image tag: alt attribute
  + audio/ video for disabilities
  + ARIA attributes

+ Placement of links:

  + HTML file parsed from top down. Pause parsing until link to external files are downloaded

  + Link CSS in header: `<link rel="stylesheet" type="text/css" href ="styles.css">`

  + Link Script

    + `async`: execute as soon as downloaded. Good if script do not depend on each other

      ```html
      <script src="script.js" async></script>
      ```

    + `defer`: async download but executed in order after entire document has been loaded

      ```html
      <script src="script.js" defer></script>
      ```

+ iframes

+ browser storage