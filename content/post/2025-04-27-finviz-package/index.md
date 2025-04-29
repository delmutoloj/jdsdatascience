---
title: "finViz Package"
author: 'James Del Mutolo'
date: '2025-04-27'
slug: finviz-package
categories: ["R"]
tags: ["finViz", "Financial", "R Package"]
---



I created the finViz (Financial Visualization) package to generate monthly financial summaries and visuals of my personal finances.

In this post, I'll outline the different components of the finViz package, walk you through how to use it, and share the process I followed to develop it.



To begin, the finViz package can be installed from GitHub using the install_github() function from devtools.

``` r
devtools::install_github("delmutoloj/finViz")
```

[GitHub Repository](https://github.com/delmutoloj/finViz)

# Datasets

The package contains two datasets...

### finViz::deposits

I maintain a spreadsheet with all deposits into my checking account. I use the read.csv function to import this dataframe into R as "deposits". This dataframe also includes employer 401k contributions so that they are appropriately accounted for in the deductions. I've included an example deposits dataset to use with the package's functions.

Deposit data contains the following columns:
- Date: Date of transaction in format YYYY-MM-DD
- Merchant: Where the transaction was made
- Category: Spending category
- Note: Note about the transaction (optional)
- Amount: Transaction total


``` r
head(finViz::deposits, 5)
```

```
##         Date              Merchant             Category Note  Amount
## 1 2025-04-11 Employer Contribution 401k: Employer Match   NA   46.93
## 2 2025-04-11            Enterprise            Gross Pay   NA 1173.15
## 3 2025-04-16                   ATM         Cash Deposit   NA   20.00
## 4 2025-04-20                Amazon               Refund   NA   92.29
## 5 2025-04-25 Employer Contribution 401k: Employer Match   NA   66.16
```

### finViz::spending

The example spending dataset contains transaction data combined from: credit card statements, a checking account statement, and payroll deductions.

This data is an output of the statementCombine() function and contains the same columns as the spending data.


``` r
head(finViz::spending, 5)
```

```
##         Date Merchant    Category Note Amount
## 1 2025-04-02   Amazon        Home   NA  55.36
## 2 2025-04-03   Panera   Fast Food   NA   9.51
## 3 2025-04-07 7-Eleven Convenience   NA   1.79
## 4 2025-04-09  Walmart   Groceries   NA  94.49
## 5 2025-04-10   Panera   Fast Food   NA   5.02
```

# Functions

### finViz::statementCombine()

The statementCombine() function is used to combine .csv files directly into a single dataframe in R. This function allows me to maintain statement spreadsheets for each of my credit cards, checking account, and deductions, and combine them all into R in a dataframe named "spending".

The function is passed the file names of the .csv files in the working directory that are to be combined.

Example:

``` r
statements <- c("credit1.csv", "credit2.csv", "checking.csv", "deductions.csv")
spending <- statementCombine(statements)
```

### finViz::financialSummary()

The financialSummary() function accepts the two dataframes, spending and deposits, and returns a list of useful information including:
- Total Income
- Total Expenses
- Net balance
- Spending by category
- Spending by merchant

Example:

``` r
financialSummary(deposits, spending)
```

```
## Total Income:  3052.46 
## Total Expenses:  2624.8 
## Net Balance (Income - Expenses):  427.66 
## 
## Spending by Category:
##           Category Amount
## 1              Car 626.42
## 2       Retirement 537.38
## 3          Savings 430.00
## 4            Taxes 397.12
## 5        Groceries 157.35
## 6        Insurance 133.18
## 7  Online Shopping 107.61
## 8             Home  55.36
## 9            Phone  55.00
## 10       Fast Food  52.47
## 11    Subscription  29.95
## 12     Convenience  20.85
## 13         Clothes  17.12
## 14   Entertainment   4.99
## 
## Spending by Merchant:
##                       Merchant Amount
## 1                 GM Financial 479.31
## 2                 Money Market 430.00
## 3                   401k: Roth 282.93
## 4                      Fed W/H 190.43
## 5                         FICA 167.51
## 6                       Amazon 162.97
## 7                        Geico 147.11
## 8                401k: Pre-Tax 141.36
## 9                      Walmart 135.66
## 10                 Medical Ins 118.18
## 11 401k: Employer Contribution 113.09
## 12                     Cashapp  55.00
## 13                      Panera  45.31
## 14                     Fed MWT  39.18
## 15                      Publix  21.69
## 16                      Platos  17.12
## 17                    7-Eleven  15.54
## 18               Amazon: Prime  15.13
## 19            Panera: Sip Club  12.83
## 20               Serenity Sips   7.16
## 21                  Dental Ins   7.06
## 22                    Racetrac   5.31
## 23                  Play Store   4.99
## 24                  Vision Ins   4.94
## 25                    Life Ins   2.14
## 26                      Google   1.99
## 27              Disability Ins   0.86
```


### finViz::financialSankey()

**Disclosure**: This function was created with significant help from ChatGPT. I will go over the specific prompts I used and offer my insight on using generative AI for coding later in this post. 

The financialSankey() function accepts deposit and spending data, and returns an interactive sankey diagram that can be used to visualize the flow of money through different categories and merchants.

This function is built with the sankeyNetwork() function from the networkD3 package.

The function groups spending categories "Taxes", "Insurance", and "Retirement" into a "Deductions" node.

Example:


``` r
# Create sankey object
sankey <- financialSankey(deposits, spending)
```

```
## Links is a tbl_df. Converting to a plain data frame.
```

``` r
# Save html
htmlwidgets::saveWidget(sankey, "sankey_temp.html")

webshot2::webshot("sankey_temp.html", file = "sankey_plot.png", vwidth = 1280, vheight = 720)
```

```
## file:///C:/Users/delmu/Documents/blogdown_site/jdsdatascience/content/post/2025-04-27-finviz-package/sankey_temp.html screenshot completed
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-7-1.png" width="672" />

I tried to get the text in the image as readable as possible, opening the image in a new tab helps. The function returns an interactive  html object that I could not get to resize correctly on this webpage, so I saved the html and made a .png with the webshot2 package to display here. Normally, I just view the sankey output fullscreen in RStudio.

# Use of Generative AI

I have been maintaining spreadsheets of my monthly finances and was wondering if it was possible to create a sankey diagram with this data in R.

So I searched the internet for a package that can create a sankey diagram and quickly found the networkD3 package.

I was curious if ChatGPT could interpret my spreadsheets and infer from the networkD3 documentation to create the visual I wanted, so I uploaded my data and made the following query.

>In R, how would I use the sankeyNetwork() function from the networkD3 package to create a sankey diagram that show the flow of income and expenses using this example data.

The code it generated did not work initially and required some error troubleshooting, but soon I was able to create a crude visual.

This process let me to familiarize myself with the specific components of the sankeyNetwork() function, allowing me to refine the code further and make more advanced queries.

I eventually ended up with a basic visual that displayed net income and expenses, but I wanted to make it more advanced and include gross income, split off into net income and deductions.

So I provided ChatGPT with the code I had at that point and made the following query:

>All deposits should be named dynamically and summed into "Gross Income". Gross Income should then split off into "Net Income" and "Deductions".
Net Income should be equal to Gross Income minus Deductions.
Deductions should split off into the categories  "Taxes", "Insurance", and "Retirement". Those categories should split off into the merchant column.
Net Income should split off into all of the dynamically named spending categories. And each of those spending categories should split off into their respective vendors.

The code generated from this query required some minor bug fixes, but otherwise had perfectly implemented what I had described.

Generative AI is a very powerful and useful tool when programming, but using it to constructively generate code requires a defined end goal, and a through understanding of the methods that will need to be used in the code you are trying to generate.

# Improvements

The main improvement I would like to work on is to add more arguments to the financialSankey() function. It would be useful to be able to modify font size, node width, node padding, and other arguments used within the function as needed.

I could also make some improvements to the financialSummary() function to have it include gross income, net income, deductions, and net balance.

This package and its functions are designed specifically for the workflow that I have developed for myself. Adding more advanced arguments to the functions in this package would allow it to be useful to more people.
