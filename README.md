# Manage HUGO taxonomies like a boss

Live view: [http://hugo-taxonomies.netlify.com/](http://hugo-taxonomies.netlify.com/)

Here you'll find a very detailed explanation on how to use taxonomies in a HUGO site. **No hackish code or intricate tricks**. There is documentation on this topic, but you'll have to search in 3 different places and some examples are missing. So it can be very confusing at times, at least it was for me.

After reading this, you will be able to:

- Create your own taxonomies
- Add custom metadata to your taxonomies
- Create a list template for taxonomy terms using all the custom metadata you added to the taxonomy
- Handle terms without a corresponding `_index.md` file and define default values in your templates
- Create a template for terms with all the data related to it
- Speed up the creation of terms pages using archetypes

## Create your own taxonomies

in config.toml

```
[taxonomies]
  brand = "brands"
```

And then list them on your pages. This is how a product page would look like:

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

## Add custom metadata to your taxonomies

Let's say you want to add a logo, website link and description for each brand. You need to place that information in your taxonomy terms pages. For example: `/content/brands/adidas/_index.md`

**Attention** Note that the structure is important. `<taxonomy>/<term>/_index.md`

```
---
title: "Adidas"
description: "This is the description for Adidas"
website: "http://adidas.com"
logo: "/images/adidas.png"
---
```

## Create an index template for taxonomy terms using all the custom metadata you added to the taxonomy (terms.html)

Now to create an index for your taxonomies, and display all the custom metadata you added for each term, you need to edit your terms template. You can add it here: `/layouts/_default/terms.html`

Range dynamically through all the term pages with `.Data.Pages`

```
<ul>
  {{ range .Data.Pages }}
    <li>
      <a href="{{ .Permalink }}">{{ .Title }}</a>
      <dl>
        <dt>Link to the brand page</dt><dd><a href="{{ .Permalink }}">{{ .Permalink }}</a></dd>

        <!-- Handling brands that don't have a corresponding page by using 'with' -->
        {{ with .Params.website }}
          <dt>Website</dt><dd>{{ . }}</dd>
        {{ end }}
        {{ with .Params.description }}
          <dt>Description</dt><dd>{{ . }}</dd>
        {{ end }}

        <!--
        Handling brands that don't have a corresponding page by using an 'if' statement.
        This let's you add default values to your template.
        -->
        <dt>Logo</dt>
        <dd>
        {{ if ne .Params.logo nil }}
          <img src="{{ .Params.logo }}" height="90" />
        {{ else }}
          <img src="http://via.placeholder.com/90x90?text=No+logo" height="90" />
        {{ end }}
        </dd>
      </dl>
    </li>
  {{ end }}
</ul>
```

## Handle terms without a corresponding `_index.md` file and define default values in your templates

You might create some terms for which you haven't created a corresponding page (`/content/brands/terms/_index.md`).

As an example, I added a product called New Balance Shoe. It's brand is "New Balance". But if you search in my content/brands folder, you won't find a `/new-balance/_index.md` file. I haven't created it.

In that case you will have to add some default parameters here. Take a look at the comments in the snippets above to find a couple ways to handle terms without a corresponding page.


## Create a template for terms with all the data related to it

Now, you can edit the template for each term page. For example, the page for the brand "Nike" would be: /brands/nike. Edit this template at: `/layouts/_default/taxonomy.html`

You can list all the pages that are labeled with the current term via `.Data.Pages`, as you can see on the example below. Just as we did before, we can handle the terms that don't have a corresponding page by using `with` or `if` statements.

```
{{ with .Params.description }}
  <p>{{.}}</p>
{{ end }}

<ul>
  {{ range .Data.Pages }}
     <li>
       <a href="{{ .Permalink}}">{{ .Title }}</a>
     </li>
  {{ end }}
</ul>
```

## Speed up the creation of terms pages by using archetypes

Having to create a `<taxonomy>/<term>/_index.md` file for each term, can be a real hassle. But you can save a lot of time by taking advantage of Archetypes. This is a **HUGE TIME SAVER**. With the setup I'll show you, whenever you want to create a new taxonomy term (brand), all you have to do is type `hugo new <taxonomy>/<term>/_index.md` on your terminal and your brand page will be created and even filled with the correct title.

Just create a new archetype for your taxonomy. `archetypes/<taxonomy>.md`. In my case, `archetypes/brands.md`

```
---
title: {{ $term := index ( split .File.Dir "/") 1 }}"{{ replace $term "-" " " | title }}"
description: "This is the description for {{ $term | title }}"
website: "http://{{ $term }}.com"
logo: "/images/{{  $term }}.jpg"
date: {{ .Date }}
draft: false
---
```

Note that I defined the variable `$term` in the first line. It takes the variable `.File.Dir`, splits it into an array, indexes it and grabs the second value. This value will serve as your title, and you can use it to dynamically create default values based on your file name. That way, when you create pages by the CLI, all you have to do is name them correctly, and all your data will be filled with the correct title.

**Try it out:**
On your terminal type:
`hugo new brands/test/_index.md`

It will use whatever value you use in the place of *test* as your title. This is what it would render:

```
---
title: "Test"
description: "This is the description for Test"
website: "http://test.com"
logo: "/images/test.jpg"
date: 2017-08-07T12:04:03-06:00
draft: false
---
```

**Important:** If your term contains special characters, for example "Ügly brand", when you type the command on your terminal, you have to remove the special characters and replace spaces with "-" (ugly-brand). Then on your `_index.md` the spaces will be taken care of, but you will have to add any special character you replaced.

**That's it**

Now you can use taxonomies like a boss. Please if stumble into any problems by using this method, don't hesitate to contact me via [the forum](https://discourse.gohugo.io/), or adding an issue on this repo.
