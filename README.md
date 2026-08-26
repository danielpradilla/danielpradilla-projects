# Daniel Pradilla Projects

Static source for [danielpradilla.info/projects](https://www.danielpradilla.info/projects/).

The page is plain HTML and CSS with no build step. It combines the visual language of the Daniel Pradilla blog and app style guide with a screenshot-led project index.

For project-selection rules, screenshot preparation, visual constraints, verification, and deployment guidance, see [AGENTS.md](AGENTS.md).

## Local preview

From this directory:

```sh
python3 -m http.server 8778 --bind 127.0.0.1
```

Open `http://127.0.0.1:8778/`.
