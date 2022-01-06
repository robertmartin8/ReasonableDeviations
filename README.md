# ReasonableDeviations

[https://reasonabledeviations.com](https://reasonabledeviations.com)

## Contents

<!-- TOC depthFrom:2 depthTo:6 withLinks:1 updateOnSave:0 orderedList:0 -->

- [Rationale](#rationale)
- [Theme and style](#theme-and-style)
	- [Fonts](#fonts)
	- [Colour](#colour)
	- [Maths](#maths)
- [Domain](#domain)
- [License](#license)

<!-- /TOC -->

## Rationale

I recently decided to make a webpage to act as both a portfolio to demonstrate some of my interesting projects, as well as a 'scientific blog' to informally log what I have been thinking about.

I do like writing in markdown – I currently have a better-than-sublime (no pun intended) setup using the delicious
[Atom text editor](https://atom.io/) which makes writing a pleasure. As such, I wanted a web platform
that allows you to write posts in markdown. Github pages with Jekyll seemed to be the right choice. I had no prior experience in HTML/CSS, but it wasn't too hard to pick up the small bits necessary.

## Installation

I followed [this guide](https://github.com/BillRaymond/install-jekyll-apple-silicon/blob/main/README.md) to set up.

## Theme and style

I decided to use the
[Hyde](https://www.github.com/poole/hyde) theme as a basis. In the author's words:

> Hyde is a brazen two-column [Jekyll](https://jekyllrb.com) theme that pairs a prominent sidebar with uncomplicated content. It's based on [Poole](https://getpoole.com), the Jekyll butler.

![Hyde screenshot](https://f.cloud.github.com/assets/98681/1831228/42af6c6a-7384-11e3-98fb-e0b923ee0468.png)

But I've changed a few minor things, including the font and the colour.

### Fonts

For the main body and all the text, I was torn between [Crimson](https://fonts.google.com/specimen/Crimson+Text?selection.family=Crimson+Text), Garamond, and Palatino.
- Crimson is marginally more informal, and it is pretty easy on the eyes. I am worried that it is not 'web-safe'.
- Palatino and Garamond are both classy, and it is hard to decide between the two. I'm leaning towards Palatino, as Garamond is a lot 'thinner'. Personal preference though.

Weighing in these considerations, my CSS looks like this:

```
font-family: "Crimson Text", Palatino, Garamond,
Times, serif;
```

For the sidebar, I decided to go with a sans-serif font. It looks cleaner and more professional. Arial was the easy option.

### Colour

The whole website is in a *lovely* shade of purple. Hyde comes packed with a choice of some base16 themes, but no thanks. I added the following css to define my purpz theme.

```css
.theme-base-purpz .sidebar {
  background-color: #2f0857;
}
/* pww 2014-12-17 - moved from poole.css to each theme */
.theme-base-purpz .content a,
.theme-base-purpz h1, h2, h3, h4, h5, h6,
.theme-base-purpz .related-posts li a:hover {
  color: #5c14a4;
}
```

Then it is just a matter of putting:

```html
<body class="theme-base-purpz">
```

into your default layout html file.


### Maths

I am using MathJax to render my maths. To get it to work, I just added the following html into my `head.html` file under my `_includes/` folder:

```html
<script type="text/x-mathjax-config"> MathJax.Hub.Config({ TeX: { equationNumbers: { autoNumber: "AMS" } } }); </script>
<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
	tex2jax: {
	  inlineMath: [ ['$','$'], ["\\(","\\)"] ],
	  processEscapes: true
	}
  });
</script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
```

## Domain

I started with the default domain, which was `robertmartin8.github.io/ReasonableDeviations`, but after a while I started to feel that this was too clunky. I decided to shop around for a nice domain, and found what I thought was a bargain on [namecheap.com](https://www.namecheap.com/). I think my new domain is costing me US$6 for the next 5 years, which is a good deal.

Namecheap offers a very useful [guide](https://www.namecheap.com/support/knowledgebase/article.aspx/9645/2208/how-do-i-link-my-domain-to-github-pages) for linking your new domain with github pages blogs, and this was actually very easy to do.

*However*, it broke a lot of the CSS and internal links because of the new url, which means that I had to go and edit all the baseurl stuff etc. So my advice for someone aspiring to make a github pages blog is to set up your custom domain before adding content. 

## License

Code for the website can be reproduced with attribution and without warranty, under the [MIT license](LICENSE.md).

Content is licensed under the Creative Commons. 
