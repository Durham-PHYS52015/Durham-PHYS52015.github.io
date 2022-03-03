Source repository for
[PHYS52015](https://www.dur.ac.uk/postgraduate.modules/module_description/?year=2020&module_code=PHYS52015).

Go to https://durham-phys52015.github.io for the hosted material.

The `.github/workflows/build.yml` shows how the material is built. You will need [hugo](https://gohugo.io), along with Python (to build some of the images), and the application version of [diagrams.net](https://app.diagrams.net) for others. If relevant prerequisites are in place then `make html` builds the images and slides and then the website.

Github actions are set up so that any push to the `main` branch is automatically built and deployed. I recommend making changes via pull requests which check that everything still builds which one can then merge.