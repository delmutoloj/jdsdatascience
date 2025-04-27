---
title: "finViz Package"
author: 'James Del Mutolo'
date: '2025-04-27'
slug: finviz-package
categories: ["R"]
tags: ["finViz", "Financial", "R Package"]
---

<script src="{{< blogdown/postref >}}index_files/htmlwidgets/htmlwidgets.js"></script>
<script src="{{< blogdown/postref >}}index_files/d3/d3.min.js"></script>
<script src="{{< blogdown/postref >}}index_files/sankey/sankey.js"></script>
<script src="{{< blogdown/postref >}}index_files/sankeyNetwork-binding/sankeyNetwork.js"></script>

In this post, I’ll outline the different components of the finViz (Financial Visualization) package, walk you through how to use it, and share the process I followed to develop it.

# Functions

### statementCombine()

Body text

### financialSummary()

Body text

### financialSankey()

**Disclosure**: This function was created with significant help from ChatGPT. I will go over the specific prompts I used and offer my insight on using generative AI for coding later in this post.

``` r
# This is an R code chunk
library(finViz)
sankey <- financialSankey(deposits, spending)
```

    ## Links is a tbl_df. Converting to a plain data frame.

``` r
sankey
```

<div id="htmlwidget-1" style="width:1920px;height:1080px;" class="sankeyNetwork html-widget"></div>
<script type="application/json" data-for="htmlwidget-1">{"x":{"links":{"source":[0,1,2,3,4,4,6,6,6,8,8,8,8,8,9,9,9,7,7,7,5,5,5,5,5,5,5,5,5,5,5,10,10,11,12,12,13,14,14,15,15,16,17,18,19,20,20,20],"target":[4,4,4,4,5,6,8,9,7,27,35,36,26,28,29,30,31,33,34,32,10,11,12,13,14,15,16,17,18,19,20,41,42,38,23,37,45,22,44,25,24,21,21,47,39,40,46,43],"value":[113.09,20,2827.08,92.29000000000001,1984.78,1067.68,133.18,537.38,397.12,7.06,0.86,2.14,118.18,4.94,113.09,141.36,282.93,167.51,39.18,190.43,626.4200000000001,17.12,20.85,4.99,52.47,157.35,55.36,107.61,55,430,29.95,479.31,147.11,17.12,15.54,5.31,4.99,45.31,7.16,21.69,135.66,55.36,107.61,55,430,15.13,1.99,12.83]},"nodes":{"name":["401k: Employer Match","Cash Deposit","Gross Pay","Refund","Gross Income","Net Income","Deductions","Taxes","Insurance","Retirement","Car","Clothes","Convenience","Entertainment","Fast Food","Groceries","Home","Online Shopping","Phone","Savings","Subscription","Amazon","Panera","7-Eleven","Walmart","Publix","Medical Ins","Dental Ins","Vision Ins","401k: Employer Contribution","401k: Pre-Tax","401k: Roth","Fed W/H","FICA","Fed MWT","Disability Ins","Life Ins","Racetrac","Platos","Money Market","Amazon: Prime","GM Financial","Geico","Panera: Sip Club","Serenity Sips","Play Store","Google","Cashapp"],"group":["401k: Employer Match","Cash Deposit","Gross Pay","Refund","Gross Income","Net Income","Deductions","Taxes","Insurance","Retirement","Car","Clothes","Convenience","Entertainment","Fast Food","Groceries","Home","Online Shopping","Phone","Savings","Subscription","Amazon","Panera","7-Eleven","Walmart","Publix","Medical Ins","Dental Ins","Vision Ins","401k: Employer Contribution","401k: Pre-Tax","401k: Roth","Fed W/H","FICA","Fed MWT","Disability Ins","Life Ins","Racetrac","Platos","Money Market","Amazon: Prime","GM Financial","Geico","Panera: Sip Club","Serenity Sips","Play Store","Google","Cashapp"]},"options":{"NodeID":"name","NodeGroup":"name","LinkGroup":null,"colourScale":"d3.scaleOrdinal(d3.schemeCategory20);","fontSize":12,"fontFamily":null,"nodeWidth":25,"nodePadding":10,"units":"","margin":{"top":null,"right":null,"bottom":null,"left":null},"iterations":50,"sinksRight":true}},"evals":[],"jsHooks":{"render":[{"code":"\n      function(el, x){\n        d3.select(el).selectAll(\".node text\")\n          .text(d => d.name + \" (\" + d3.format(\"(.0f\")(d.value) + \")\");\n      }\n    ","data":null}]}}</script>
