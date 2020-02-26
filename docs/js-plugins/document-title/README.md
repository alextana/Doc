# Dynamic Page Meta Title
## Looks to be SEO Friendly as well [Link](https://webmasters.stackexchange.com/questions/116411/is-a-dynamic-page-title-tag-updated-by-javascript-bad-for-seo)

## Plugin
``` js
function setDocumentTitle() {
	var getTitle;
	getTitle = document.getElementsByTagName('h1')[0].textContent;
	document.title.indexOf('###') > -1 || document.title.indexOf('#pagetitle#') > -1 ? document.title = ` `+ getTitle +` | your content | sitename` : null;  
}
```

The scripts gets the h1 tag in the page and sets it as the first part of the title, the rest of it is separated by "|" and needs to be edited in the script

## Why it can be useful?
Imagine the locations change, we often include the locations in meta titles for SEO where `your content` is in the script, you'd have to change it once on this script and it will be affecting the whole site.

## How to use?
* Copy and paste the plugin code in your global.js
* Change "your content" and "sitename" values 
* Call the function inside docready using `function setDocumentTitle();`
