# homepage

A simple POSIX shell script that helps me maintain my homepage.

## Purpose

I write all articles on my homepage in Markdown. Then I convert them to HTML.
When I have edited an existing article, I compare the new version with the
old (published) version. Finally, I publish my new/updated article by copying
it to the right (sub)folder of my homepage (which is mounted via `sshfs`).

For all these tasks I use `homepage`:

* `homepage convert my-article.md`
* `homepage compare my-article.html`
* `homepage publish my-article.html`

## Configuration

The configuration is read from `~/.config/homepage/config` if available
or from `config` in the `homepage` script directory otherwise.

Set all variables in your copy (or the original otherwise) to your needs.

When you use `homepage convert FILE`, the created HTML output is pasted
into an HTML template which contains HTML metadata, a header and a footer,
links a CSS file and possibly JavaScript files.

The path to the HTML template is set in the conifguration file (`TEMPLATE=`).
Otherwise, `homepage` searches for `TEMPLATE.html` in

1. the current working dirctory,
2. the root directory of the local copy of the homepage (`$LOCAL`),
3. `$HOME/.config/homepage/`
4. in the directory of the `homepage` script.

`homepage convert` calls [md2html](https://github.com/mity/md4c) which needs
to be in your `PATH`.

## Markdown metadata

`homepage convert` expects Markdown files that start with a comment/header
like this:

```markdown
[//]: # (
Title: st as an Alacritty replacement
Description: How to configure st coming from Alacritty
Date: 2021-08-01
Update: 2022-12-17
)
```

* `Title`: Used as HTML title, [Open Graph protocol](https://ogp.me/) title and `H1` header. The OG title is restricted to 35 characters.
* `Description`: Used as HTML metadata and Open Graph description. The latter is restricted to 65 characters.
* `Date`: Date of creation, inserted as first paragraph (depending on the template). If missing, current (conversion) date is used.
* `Update`: Date of last modification, inserted next to the date of creation (dependiong on the template). Optional.

Since the default template uses `H1` header for the article title, I use only `H2` to `H6` in my Markdown files.

## Custom tag attributes

When a Markdown file contains a comment like this:

```markdown
<!-- class="number-table striped-table" -->
```

… `homepage convert` will add these attributes to the following tag in the resulting HTML file.
