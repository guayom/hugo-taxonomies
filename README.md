# Hugo taxonomies example

This is how I'm using taxonomies and pages together in hugo to add custom metadata to taxonomies.

Here's the live view [http://hugo-taxonomies.netlify.com/](http://hugo-taxonomies.netlify.com/)

## How does it work?

- I create a `/data/<TAXONOMY>/<TERM>.yml` file for every taxonomy term.
- Then, whenever I want to get the data from a taxonomy, I pull the data from that data file.
Like so:

```
<ul>
  {{ range $key, $taxonomy := .Site.Taxonomies.brands }}
    <li>
      <a href="/brands/{{ $key }}">{{ title $key }}</a>
      {{ range $.Site.Data.brands }}
        {{ if eq .title (title $key) }}
          <dl>
            <dt>Description</dt><dd>{{ .description }}</dd>
            <dt>Website</dt><dd>{{ .website }}</dd>
          </dl>
        {{ end }}
      {{ end }}
    </li>
    <ul>
        {{ range $taxonomy.Pages }}
        <li hugo-nav="{{ .RelPermalink}}"><a href="{{ .Permalink}}"> {{ .LinkTitle }} </a> </li>
        {{ end }}
    </ul>
  {{ end }}
</ul>
```
