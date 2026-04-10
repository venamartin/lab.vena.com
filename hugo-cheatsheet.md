# Hugo Personal Cheatsheet

This is a personal reference file. Since it is located in the root directory (outside of `content/` or `static/`), Hugo will completely ignore it during the build process, so it won't be generated or published to your live site!

## Comments

**HTML Comment** (renders in generated HTML source code):
```html
<!-- This is an HTML comment -->
```

**Hugo Comment** (stripped entirely from build):
```go
{{/* This is a Hugo template comment */}}
```

## Adding Images

**Hugo Figure Shortcode** (with Beautiful Hugo / Easy Gallery):
```markdown
{{< figure src="/images/meshcore/example.jpg" width="100px" >}}
```
*Note: Make sure to include the unit (`px` or `%`) on the width so the underlying CSS parses it correctly!*

## Development Commands

**Start the local development server:**
```bash
hugo server -D
```
*(`-D` includes drafts)*

**Build the final site to the /public folder:**
```bash
hugo
```

## Media Embedding

**Embed a YouTube Video:**
```markdown
{{< youtube VIDEO_ID >}}
```
*Example: For `https://youtu.be/whUxTncxVGs`, the ID is `whUxTncxVGs`, so you would write `{{< youtube whUxTncxVGs >}}`.*
