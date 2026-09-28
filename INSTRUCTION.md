# Website editing guide

Quick reference for editing this al-folio (Jekyll) site.

- Page files live in `_pages/`
- Site-wide settings live in `_config.yml`
- The site is served under `/portfolio` (`baseurl` in `_config.yml`), so local URLs always end in `/portfolio/`

---

## 1. Open a live demo while you edit

A live demo rebuilds the site every time you save a file, so you can see your changes in the browser.

### Option A: Ruby on your Mac (what this folder is set up for)

Open a terminal in the `portfolio` folder and run:

```bash
cd ~/Documents/VirginiaTech/portfolio
bundle install                         # only needed the first time, or after the Gemfile changes
bundle exec jekyll serve --livereload
```

When it prints `Server running...`, open **http://localhost:4000/portfolio/** in your browser.

- Save a file and the page refreshes by itself (thanks to `--livereload`).
- **If you edit `_config.yml`, the live demo does not pick up the change.** Stop the server with `Ctrl + C` and run `bundle exec jekyll serve --livereload` again.
- To stop the demo, press `Ctrl + C` in the terminal.

### Option B: Docker (if the Ruby setup gives you errors)

Start Docker Desktop first, then run:

```bash
cd ~/Documents/VirginiaTech/portfolio
docker compose pull      # only needed the first time
docker compose up
```

Open **http://localhost:8080/portfolio/**. The first start downloads about 400 MB. Docker restarts the site by itself when `_config.yml` changes. Press `Ctrl + C` to stop.

### Troubleshooting

| Problem                                    | Fix                                                                                           |
| ------------------------------------------ | --------------------------------------------------------------------------------------------- |
| The page is blank or shows "404"           | Make sure the URL ends in `/portfolio/`                                                       |
| "Address already in use"                   | A server is still running in another terminal. Stop it, or run `lsof -i :4000` to find it     |
| A change doesn't show up                   | Hard-refresh the browser (`Cmd + Shift + R`). If you edited `_config.yml`, restart the server |
| The build acts strangely after big changes | Stop the server, run `bundle exec jekyll clean`, then start it again                          |

---

## 2. Show or hide a page

Each file in `_pages/` starts with a block between two `---` lines, called the front matter. It controls whether the page shows up in the top menu.

Example (`_pages/projects.md`):

```yaml
---
layout: page
title: projects
permalink: /projects/
nav: true # true = shown in the top menu, false = hidden from the menu
nav_order: 3 # position in the menu (1 = first, left-most)
---
```

### Show a page in the menu

Set `nav: true` and pick a `nav_order`.

### Hide a page from the menu (it still exists at its link)

Set `nav: false`. The page is still built, so anyone with the direct link (for example `/portfolio/blog/`) can open it.

### Remove a page from the site completely

Add `published: false` to the front matter:

```yaml
---
layout: page
title: teaching
permalink: /teaching/
published: false # page is not built at all; its link gives a 404
---
```

Delete that line (or set it to `true`) to bring the page back.

### Current pages

| File              | Menu title        | In menu? | Order |
| ----------------- | ----------------- | -------- | ----- |
| `about.md`        | about (home page) | always   | —     |
| `publications.md` | publications      | yes      | 2     |
| `projects.md`     | projects          | yes      | 3     |
| `repositories.md` | repositories      | yes      | 4     |
| `cv.md`           | CV                | yes      | 5     |
| `teaching.md`     | teaching          | no       | 6     |
| `profiles.md`     | people            | no       | 7     |
| `dropdown.md`     | submenus          | yes      | 8     |
| `blog.md`         | blog              | no       | 1     |
| `books.md`        | bookshelf         | no       | —     |
| `news.md`         | news              | no       | —     |
| `plugins.md`      | plugins           | no       | —     |

The `dropdown.md` "submenus" page is a demo menu that links to bookshelf and blog. Set `nav: false` on it if you don't want it.

> Tip: after hiding or showing pages, check that the remaining `nav_order` numbers still put the menu in the order you want. Gaps (2, 3, 5) are fine.

---

## 3. Add a new page

Example: adding a **Research** page.

### Step 1: Create the file

Create `_pages/research.md`:

```markdown
---
layout: page
title: research
permalink: /research/
description: What I work on and why.
nav: true
nav_order: 3
---

## Current work

Write your content here in normal Markdown.

- Bullet points work
- **Bold** and _italic_ work
- Links: [Virginia Tech](https://www.vt.edu)

## Images

Put images in `assets/img/`, then add them like this:

{% include figure.liquid loading="eager" path="assets/img/my_image.jpg" class="img-fluid rounded z-depth-1" %}
```

What each front-matter field does:

| Field          | Meaning                                                                                                              |
| -------------- | -------------------------------------------------------------------------------------------------------------------- |
| `layout: page` | Use the standard page design. Keep this for normal pages                                                             |
| `title`        | Name shown in the menu and at the top of the page                                                                    |
| `permalink`    | The page's address: `/research/` becomes `.../portfolio/research/`. Must be unique and should start and end with `/` |
| `description`  | Short line shown under the title (optional)                                                                          |
| `nav`          | `true` to show it in the top menu                                                                                    |
| `nav_order`    | Where it sits in the menu                                                                                            |

### Step 2: Fix the menu order (if needed)

If you gave the new page an order that another page already uses (like `3` above), bump the others up by one: `projects.md` → 4, `repositories.md` → 5, `cv.md` → 6, and so on.

### Step 3: Check it in the live demo

With the live demo running (section 1), open **http://localhost:4000/portfolio/research/**. The new tab should also appear in the top menu. A brand-new file sometimes needs a server restart before it shows up.

### Optional: put the page under a dropdown menu

Instead of its own tab, you can list the page inside the "submenus" dropdown by editing `_pages/dropdown.md`:

```yaml
children:
  - title: research
    permalink: /research/
  - title: divider
  - title: blog
    permalink: /blog/
```

In that case set `nav: false` in `research.md` so it doesn't also appear as a separate tab.

---

## 4. Hide content without deleting it

### Parts of a page (`{% include %}` lines, etc.)

Wrap them in a Liquid comment block. Everything inside stays in the file but isn't built or shown:

```liquid
{% comment %}
  {% include calendar.liquid calendar_id='test@gmail.com' timezone='Asia/Shanghai' %}

  {% include courses.liquid %}
{% endcomment %}
```

To bring them back, delete the `{% comment %}` and `{% endcomment %}` lines.

- Don't use an HTML comment (`<!-- ... -->`) for this. Jekyll still runs the `{% include %}` lines inside it, so the hidden content ends up in the page's HTML source.
- Example: `_pages/teaching.md` hides the calendar and course list this way. The course pages in `_teachings/` are still built at their own addresses; add `published: false` to each of those files to hide them too.

### Publications in `_bibliography/papers.bib`

BibTeX has no block comments. Instead, change the entry type to `@comment`:

```bibtex
@comment{einstein1920relativity,      <- was @book{einstein1920relativity,
  title  = {Relativity: the Special and General Theory},
  ...
}
```

To bring an entry back, change `@comment` back to its original type (`@article`, `@book`, `@inproceedings`, ...).

To do this for many entries at once, select them in VS Code, open find and replace (`Cmd + Option + F`), and turn on "Find in Selection" (`Option + Cmd + L`). Then replace `@article{` with `@comment{`, then `@book{` with `@comment{`.

Or move the entries into a separate file such as `_bibliography/template_papers.bib`. The site only reads `papers.bib`, so the other file is ignored.

---

## 5. Highlight my name in publication author lists

### Which name counts as "me"

Set in `_config.yml`, under `scholar:`:

```yaml
scholar:
  last_name: [Nguyen]
  first_name: [Tien, T.]
```

An author in `papers.bib` is highlighted only when **both** the last name matches and the first name exactly matches one of the `first_name` values. So write yourself as `Nguyen, Tien` or `Nguyen, T.`. If you use another spelling, such as `Tien K.`, add it to the `first_name` list.

Restart the live demo after changing `_config.yml`.

### How the highlight looks

The theme's default is only a thin underline. This site overrides it so your name looks like inline `code`: a monospace font in the theme color on the gray code background.

The style lives at the **bottom of `assets/css/main.scss`**. That file is a copy of the theme's main stylesheet with a "Local customizations" section added at the end. Edit only that section.

> Don't create a `_sass/` folder for custom styles. The template's "Integration tests" GitHub check rejects it and shows a red ❌ (the site still deploys, but the check keeps failing).

The rule:

```scss
.publications ol.bibliography li .author > em {
  font-family: var(--bs-font-monospace, SFMono-Regular, Menlo, Monaco, Consolas, monospace);
  font-size: 0.82em;
  font-style: normal;
  color: var(--global-theme-color);
  background-color: var(--global-code-bg-color);
  border-bottom: none;
  border-radius: 3px;
  padding: 2px 4px;
}
```

Ideas for other looks (edit the rule above):

- **Bold instead:** replace the whole body with `font-weight: bold; font-style: normal; border-bottom: none;`
- **Keep the normal font but keep the gray box:** delete the `font-family` and `font-size` lines
- **Different color:** change `color`, for example `color: #861f41;` (Virginia Tech maroon)

To go back to the theme's default underline, delete `assets/css/main.scss`.

After updating the theme to a new version, run `bundle exec al-folio upgrade overrides audit` to check whether the theme's own `main.scss` changed and your copy needs updating.

### Add other custom styles

Put any other CSS tweaks in the "Local customizations" section at the bottom of `assets/css/main.scss`. It loads last, so its rules win over the theme's rules when both selectors are equally specific.

---

## Publish your changes

The live demo is only on your computer. To update the real site (https://tiensoare.github.io/portfolio/), commit and push:

```bash
git add .
git commit -m "Describe what you changed"
git push
```

GitHub Actions rebuilds the site. Check the **Actions** tab on GitHub; the site updates a few minutes after the run turns green.
