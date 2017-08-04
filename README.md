# Hugo taxonomies example

This is how I'm using taxonomies and pages together in hugo to add custom metadata to taxonomies.

Here's the live view [http://hugo-taxonomies.netlify.com/](http://hugo-taxonomies.netlify.com/)

## How does it work?

Pages are responsible for holding content
Taxonomies are responsible for organizing content

Hugo is smart enough to create a page for each taxonomy term. That is a powerful feature and a great time saver. But if you need to attach content to a taxonomy, and using the responsibility distribution described before, the proper place to put it is in a page. So the idea here is to override the default /taxonomy/term page that hugo creates for you.

- In brands, I created a file for every brand. Those are just regular pages.
- In the brands index, instead of using the default taxonomy list view, I used a section view, since this is a regular section
- In products I used taxonomies

## The benefit

For every taxonomy you have a matching page that contains additional meta data. When you need to show taxonomies metadata, you can get it from their matching pages.
