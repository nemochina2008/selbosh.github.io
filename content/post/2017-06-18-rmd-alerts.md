---
title: Pretty errors, warnings and messages in R Markdown
slug: rmarkdown-alerts
date: '2017-06-18T13:15:00+01:00'
categories: ['R']
images: ['/img/2017/alerts.png']
---

When knitting an R Markdown document to HTML output, R chunks can produce warnings, errors or messages.

Normally these messages look like any other console output:

![R Markdown alert messages](/img/2017/alerts2.png)

Pretty ugly, and usually something I find myself trying to hide at the earliest opportunity.

But if you're using R Markdown's default template, which uses [Twitter Bootstrap](http://getbootstrap.com/), you can promote warnings, errors and messages to first-class citizens.

What if you could have them looking like this?

![Bootstrap-styled alert messages](/img/2017/alerts.png)

Bootstrap includes [dedicated message boxes](http://getbootstrap.com/components/#alerts) for danger, warnings and information.
If you were writing an ordinary web site, you would generate these using the following HTML markup:
```html
<div class="alert alert-danger"
  <strong>Danger:</strong> This is a warning!
</div>
```

You can achieve this in R Markdown using knitr chunk hooks.
Just add the following code to a chunk near the start of your `.Rmd` file.

```r
knitr::knit_hooks$set(
   error = function(x, options) {
     paste('\n\n<div class="alert alert-danger">',
           gsub('##', '\n', gsub('^##\ Error', '**Error**', x)),
           '</div>', sep = '\n')
   },
   warning = function(x, options) {
     paste('\n\n<div class="alert alert-warning">',
           gsub('##', '\n', gsub('^##\ Warning:', '**Warning**', x)),
           '</div>', sep = '\n')
   },
   message = function(x, options) {
     paste('\n\n<div class="alert alert-info">',
           gsub('##', '\n', x),
           '</div>', sep = '\n')
   }
)
```
And you're done! For a full demonstration and further details, read [this vignette](http://selbydavid.com/vignettes/alerts.html).