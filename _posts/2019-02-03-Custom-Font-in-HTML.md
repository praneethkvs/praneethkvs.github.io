---
layout: post
title: Using a custom Font in HTML/CSS
---

To use a custom font in your HTML page, download the font, most preferably in ".woff" and ".woff2" formats as these render on almost 
all the common web browser available today. If you don't have it in this format, you can upload your font here and download a webkit.  
https://www.fontsquirrel.com/tools/webfont-generator

Add the following code to your CSS,
```css
@font-face {
  font-family: 'MyWebFont';
  src:  url('myfont.woff2') format('woff2'),
        url('myfont.woff') format('woff');
}
```

and to use it.
```css
body {
  font-family: 'MyWebFont', Fallback, sans-serif;
}
```
Ref: https://css-tricks.com/snippets/css/using-font-face/
