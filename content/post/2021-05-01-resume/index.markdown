---
title: 'Outstanding Resume with RMarkdown in 30 min'
author: 'admin'
date: '2021-08-01'
slug: []
categories:
  - R
tags:
  - RMarkdown
  - resume
  - blogdown
  - PDF
  - tutorials
  - CSS
  - Rmd
  - YALM
  - knitr
subtitle: ''
summary: ''
authors: []
lastmod: '2021-08-01T11:15:13+08:00'
featured: no
disable_jquery: yes
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
---

R has been updating drastically in recent years. Different packages will help you with the right tools (YAML, CSS, js, fonts) to get sophisticated outputs. We will use `pagedown` as a package to design different documents, among them **resumes**. To make it even easier, efficient, and fast we will use an excellent template and add some modifications to give it a unique style.

Major points that you will learn:

-   Pagedown
-   RMarkdown
-   CSS

## Installation

Like any other package, we will need to install the package. You can add the following line of code in your console, R file, RMarkdown file. Similarly, you can go to the *Packages* window and type `pagedown`.


```r
install.packages("pagedown",dependencies = T) # add any required package
library(pagedown)
```

After successfully installed the packages, we will be almost ready to create a resume.

## Preliminary output

Go to **File-\> New File-\> R markdown -\> From template -\> HTML resume {blogdown}**

At this point, you can use the **knit** bottom, and you will the default template. There are many parts inside the document we could explain all of them, but you will learn faster if you have something that looks and works right. I have used this approach in many situations; you will avoid minor or straightforward errors.

## Modifications

After seeing the preliminary output, you can correctly identify the information inside the .Rmd file. To change the image, you need to add the full path to the image, or place it where you saved your .rmd file to use just the name. Something like this will work:

    ![caption](my_image.jpg).

If you are happy with the style and content, that will be everything you need to do. *How fast and awesome was that !!!* No need to spend a whole day with Word.

After you changed the text, you may want to add a few additional points to style it slightly differently.

### YALM

We will need a new `.css` file to store all the additions in our Resume. Use Rstudio to create the file by using **File -\> New File -\> CSS** file and save it. I named it `changes.css`; name it as you wish and follow the template below. Everything that we add here will be joined with the original `.css` after the following text in the YAML header of your .Rmd file. The YAML header is at the top of your document. To use our modified CSS file then you need the following. Knit it again to add the css files. Later you only need to add your css modifications, save, and refresh the pop-up window (top corner, to the right of `publish`).

    date: "2021-05-09"
    output:
      pagedown::html_resume:
        css:
          - changes.css         # OVERIDE CERTAIN FUNCTIONS OF CSS
          - resume               # DEFAULT FILE
        # set it to true for a self-contained HTML page but it'll take longer to render
        self_contained: false

> In all your RMarkdown documents, you need to make sure the indent is adequate; otherwise, it will not work, or you will get errors.

If the `viewer` window is too small, click the third icon at the top-left corner of the panel (show in a new window). You can also copy the link appearing in the R Markdown window after you knit the document. It shows something like "serving ..... at <http://something124>....".

### CSS

The main thing that you need to know is how to identify the structure of the document. With the latest version (0.14), you will get some additional folders, including font, CSS, and other attributes. Go to the folder `resume_files` -\> `paged-0.14` -\> `css` and open resume.css. That one has all the information that you currently have. If you want to modify something, we will copy and add it to our new CSS file. If we want to add something new, we follow the guides used in the corresponding parts in the default file.

> Select your `chages.css` file and let's add some awesome changes.

To override default settings inside the root, add your modification inside `{ }` with `*` as the name. Something like this:

    * {
      /* Override default margins*/
      --pagedjs-margin-right: 0.2in;
      --pagedjs-margin-left: 0.2in;
      --pagedjs-margin-top: 0.2in;
      --pagedjs-margin-bottom: 0.2in;
      --sidebar-width: 12rem;
      --sidebar-background-color: #D8F0FF;
      --decorator-border: 2px solid #a2e2e2; /* change color and thickness of timeline */
    }

If you want to justify all the text, then you can have something like this:

    body {
        text-align: justify;
     }

If you want to do this change in one place then use `divisions` with specific styles `<div style="text-align:justify;">` some text that will be justified `</div>`.

We will have more information to add to the sidebar or the body. We can do something like this to maintain the style.

-   Identify the code inside the default CSS
-   Rename it accordingly, the same in both the .Rmd and changes.css.

I want to add **languages**, so I did the following:

        Languages {#languages}
     -----------------------------------------------------------------------
      - <i class="fa fa-language"></i> Add your text


Notice that we added `fa-language` that is a great icon <i class="fa fa-language"></i> from [Font Awesome](https://fontawesome.com/) for this part. You can find the complete list [here](https://fontawesome.com/icons?d=gallery&p=2).

## Summary

* Create a resume using blogdown template ????.

* Replace the content with your information ???	.

* Add new categories following the current structure ????.

* Change the style by adding a new CSS file ???.

***

These were some of the steps that I followed to create my {{< staticref "media/resume.pdf" "newtab" >}}resum??{{< /staticref >}}. Hopefully, you add different modifications to get excellent results ????. If you liked this post, share it with your friends and on your social media. Constantly check my page for more practical and efficient R tutorials.
