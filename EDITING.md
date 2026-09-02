# How to change the words on your website

All the text on the site lives in **one file**. You don't need to know any code —
you just change the words and press a button.

**→ [Click here to edit the words](https://github.com/hillelcoren/chana-vered.github.io/edit/main/content.yml)**

---

## The three steps

1. **Click the link above.** You'll see the text of your site in a plain editor.
2. **Change whatever you like.** Only change the actual words — see the two rules below.
3. **Scroll to the bottom and press the green "Commit changes" button.** A box pops
   up; you can ignore it and press the green button again.

Wait about a minute, then refresh your website. Your new words will be there.

> **First time only:** Hillel needs to add you as a collaborator on the repository
> (Settings → Collaborators). Without that, GitHub won't let you press the green
> button — it'll ask to send a suggestion instead.

---

## The two rules

Each piece of text looks like this:

```yaml
  heading: >-
    A teacher who begins with the human being in front of her.
```

**Rule 1 — don't touch the top line.** `heading: >-` is the label that tells the
site where this text goes. Leave it exactly as it is, including the `>-`.

**Rule 2 — keep your words indented.** Your text sits on the line(s) underneath,
pushed in from the left by a few spaces. Keep that spacing.

That's it. Inside your own text you can type anything you like — apostrophes,
quotation marks, dashes, colons, `&` — nothing will break.

Long paragraphs can run across several lines. They get joined back together into
one paragraph on the website, so you can break lines wherever it's comfortable:

```yaml
  intro: >-
    Chana Vered speaks at shuls, schools, women's events and conferences —
    in person and online. Talks are shaped around your audience,
    not delivered from a shelf.
```

---

## Where things are

The file is in the same order as the page, top to bottom:

| Section | What it controls |
|---|---|
| `site` | The browser tab title, and the description Google shows |
| `nav` | The menu at the top |
| `hero` | The opening screen: the tagline, the big headline, the buttons |
| `stats` | The four small cards with the big numbers |
| `about` | The About section and the quote in the coloured box |
| `teaching` | "Four ways to learn together" and its four cards |
| `speaking` | The speaking section, your talk topics, and the big card on the right |
| `projects` | The Projects section — each project has a logo, tagline, headline, description and link |
| `testimonials` | The quotes that appear one after another as you scroll |
| `contact` | The dark section at the bottom — including **your email address** |
| `footer` | The very bottom line |

**Your email address** appears in two places on the site but you only change it in
one place — under `contact`, next to `email`. Both update together.

### Adding or removing an item

The stat cards, teaching cards, talk topics, projects and testimonials are lists. Each item
starts with a `-`. To add one, copy an entire block (from its `-` down to the line
before the next `-`) and paste it below, then change the words. To remove one,
delete its whole block. The spacing and animations sort themselves out.

One exception: a **project** needs all six of its lines filled in, including `logo`.
If you want to add a project, ask Hillel to add its logo image first.

---

## If something goes wrong

**Your website will not break.** If there's a mistake in the file, the site simply
keeps showing the last good version, and GitHub emails you to say the update
didn't go through. Fix the file, commit again, and it'll publish.

The most likely mistake is losing the indentation on a line, or accidentally
deleting a label like `heading: >-`. If you get stuck, GitHub keeps every previous
version of the file — Hillel can restore it in a few seconds.

---

## For the developer

The site is a Jekyll build on GitHub Pages — no workflow file, no build script.
`content.yml` is the only content source; `index.html` is the template.

- `_config.yml` sets `data_dir: .`, which is what lets `content.yml` sit at the repo
  root instead of in a `_data/` folder while still being read as `site.data.content`.
  It's also excluded there so it isn't copied into the built site.
- `liquid.strict_variables` / `strict_filters` are on, so a missing key fails the build
  loudly (naming the key and line) instead of silently publishing a blank spot. A
  failed build leaves the live site on its last good version.
- Every value is emitted through `| escape`, so typed `<`, `>` and `&` render as
  text and can't inject markup.
- Every value in `content.yml` uses a `>-` block. This is load-bearing, not style: the
  real copy contains colons (`disciplines woven together: …`, the `1:1` stat) that
  would break a plain YAML scalar.
- **Local preview.** `index.html` contains Liquid, so serving the repo root directly
  (Valet at `chana-vered.github.io.test`) shows raw template syntax. Build first:

      jekyll build -d /tmp/cv-site

  and point Valet at `/tmp/cv-site`, or use `jekyll serve` → `localhost:4000`. There is
  deliberately no `Gemfile`: Jekyll auto-loads bundler whenever one is present, which
  breaks the build unless the `github-pages` bundle is installed locally.
