# Hugo taxonomies example

Live view: [http://hugo-taxonomies.netlify.com/](http://hugo-taxonomies.netlify.com/)

## The problem

I wanted to add custom metadata to taxonomies. There is not a lot of documentation on how to do this. So I came up with this technique.

## Solution

Let's say I have a shoes website. My main content would be product pages with the metadata for the product. My main taxonomy would be brand, in order to be able to assign a brand to each product.

in config.toml

```
[taxonomies]
  brand = "brands"
```

This is how a product page would look like:

```
---
title: "Adidas Ligra 5"
description: "This is the description for Adidas Ligra 5"
brands:
  - Adidas
date: 2017-08-05T07:43:13-06:00
draft: false
---
```

Now, when I visit `/brands/` I want a to display all my brands, but also gives a little more information about each one, like a logo, a description and the brand's official website link. To store the information for each brand, I create a page for each one. For example:

`/content/brands/nike/_index.md`

```
---
title: "Nike"
description: "This is the description for Nike"
website: "http://nike.com"
logo: "/images/nike.png"
---
```

**Attention:** The folder structure is important: `/content/<taxonomy>/<term>/_index.md`

Then all I need to do is list these pages in my taxonomyTerms template (`/layouts/default/terms.html`). But grouping those pages can be tricky and confusing. What you want is to be able to treat `/brands` almost like a section. But since the pages are nested, you can't really do it. So here's my little trick. I created another taxonomy for the brands pages. Here's how it looks in my config.toml

```
[taxonomies]
  brand = "brands"
  taxonomy_index = "taxonomy_indexes"
```

And now, I use it on my brands pages:
```
---
title: "Nike"
description: "This is the description for Nike"
website: "http://nike.com"
logo: "/images/nike.png"
taxonomy_indexes:
  - "brands"
---
```

Finally, I just list them in a breeze like this on my taxonomyTerms template (`/layouts/_default/terms.html`):

```
<ul>
  {{ range .Site.Taxonomies.taxonomy_indexes.brands }}
    <li>
      <h3>{{ .Title }}</h3>
      <dl>
        <dt>Link to the brand page</dt><dd><a href="{{ .Permalink }}">{{ .Permalink }}</a></dd>
        <dt>Website</dt><dd>{{ .Params.website }}</dd>
        <dt>Description</dt><dd>{{ .Params.description }}</dd>
        <dt>Logo</dt><dd><img src="{{ .Params.logo }}" height="90" /></dd>
      </dl>
    </li>
  {{ end }}
</ul>
```

There, now I can add custom metadata to my brands and list those pages in a very simple way.

## Drawbacks:

Hugo will also generate pages for taxonomy_indexes.

The solutions I can think of are:

- Redirect those pages
- Use a gulp or other kind of automation to delete those pages after the build.

Please, if you can think of any better solution to this, please please enlighten me.
