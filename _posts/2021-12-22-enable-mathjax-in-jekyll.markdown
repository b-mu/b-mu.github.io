---
layout: post
title:  "How to enable MathJax in Jekyll minima theme"
date:   2021-12-22 00:21:00 -0400
categories: jekyll update
---

1: Find the minima bundle
```
bundle show minima
```

2: Inside the bundle (given by step 1), add the code block below to the end of ```_layouts/default.html```  (i.e. after the outmost ```<html> ... </html>```)

```html
<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    extensions: ["tex2jax.js"],
    jax: ["input/TeX", "output/HTML-CSS"],
    tex2jax: {
      inlineMath: [ ['$','$'], ["\\(","\\)"] ],
      displayMath: [ ['$$','$$'], ["\\[","\\]"] ],
      processEscapes: true
    },
    "HTML-CSS": { fonts: ["TeX"] }
  });
</script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript">
</script>
```
* note:
	* this step enables MathJax (with both inline and display mode)
		* ```$$...$$``` is only rendered as display math if the lines above and below it are blank
	* the above code uses the copy of MathJax from a Content Delivery Network [cdnjs](https://cdnjs.com/), which needs network access. alternative: download and install a local copy of MathJax on server/hard disk

3: Create a local copy of ```_layouts``` and save the change
```
cp -r {path to _layouts in the bundle} {path to repo}
```

# Reference:
* [MathJax v2.7 docs: Loading and Configuring MathJax](https://docs.mathjax.org/en/v2.7-latest/configuration.html)
* [Blog: Use MathJax to write Equations in Jekyll blogs](http://zjuwhw.github.io/2017/06/04/MathJax.html)