# Routing Notes

### Introduction 

In traditionals websites before, each page was completely separate and distinct from others and every page was served independently from the server. When you requested `home.html`, the browser asked to the server for the `home.html` file, which the server would then return to the browser and then display. And if you went to `profile.html`, the browser would ask the server for that file and the file would be returned and then displayed and the *entire* page would be replaced.

In SPAs, you load a file in memory `index.html`, and then the rest of the pages are loaded via JavaScript. It means that only a few portions of `index.html` are replaced as you navigate from page to page. 

