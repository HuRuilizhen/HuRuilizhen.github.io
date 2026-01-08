# Ray's Blog

The site is built with [Jekyll](https://jekyllrb.com/) using a customized
[Scriptor](https://github.com/JustGoodThemes/Scriptor-Jekyll-Theme) theme.

It serves as a repository for technical notes and write-ups from study,
work, and hands-on experimentation.

Live site: [https://huruilizhen.github.io](https://huruilizhen.github.io)

---

## Content Organization

The site is used to host various write-ups accumulated during study, work,
and hands-on experimentation, and is maintained as a long-term personal record.
Content is written primarily for personal reference and gradual refinement,
rather than as polished tutorials. The content is roughly organized into
the following categories:

- **Engineering Notes & Projects**  
  Design notes, implementation details, and reflections from practical software
  work.

- **Tools & Experiments**  
  Small utilities and exploratory implementations used to understand APIs,
  system behavior, or specific technical problems.

- **Study / Research / Work Notes**  
  Notes derived from courses, reading, and work experience.

- **Environment & Configuration**  
  Development environment setup, tooling configuration, and workflow records.

---

## Development

This site can be previewed locally using Docker.

For macOS / Linux user:

```bash
docker run --rm \
  --volume="$PWD:/srv/jekyll:Z" \
  --volume="$PWD/vendor/bundle:/usr/local/bundle:Z" \
  -p 4000:4000 \
  -it jekyll/jekyll \
  jekyll serve --host 0.0.0.0
```

For Windows user:

```powershell
docker run --rm `
  -v "$(pwd):/srv/jekyll" `
  -v "$(pwd)/vendor/bundle:/usr/local/bundle" `
  -p 4000:4000 `
  -it jekyll/jekyll `
  jekyll serve --host 0.0.0.0
```

After startup, visit [localhost:4000](http://localhost:4000) to verify the content.

---

## Deployment

The site is deployed via GitHub Pages.

---

> [!NOTE]
> This repository mainly contains website source files and personal technical notes.
> Content may be incomplete and subject to change.
