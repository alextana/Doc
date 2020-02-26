# Set Brand Links for Components 
Helper function that adds the franchise slug to links that have a chosen class
**Plugin requires babel to work on IE11 as it needs to be compiled.**

# What does it do?
This plugin adds the franchise slug to links that have a specific class chosen when calling the function.
## When is it useful?
Whenever you want to build a component for various franchises and you don't have access to a manufacturer literal, for example a Useful Links section that you can reuse.
# How To Use
The initial setup requires you to add a global array where you store all the franchises on the website (it's highly recommended to put this in your global.js file)
``` js
let siteFranchises = ['skoda', 'citroen', 'mercedes-benz', 'abarth'];
```
## Plugin
``` js
const setBrandLink = (linkClass, franchises) => {
  // loop through the links that need to have the franchise
  let links, franchiseToUse;
  links = document.querySelectorAll(linkClass);
  if(links) {
    // check which franchise to use
    for(var x=0;x<franchises.length;x++) {
      // check if the first parameter of the url contains a franchise name 
      if(location.pathname.split('/')[1].indexOf(franchises[x]) > -1) {
        franchiseToUse = franchises[x];
      }
    }
    for(var i=0;i<links.length;i++) {
      let getLink, newLink;
      getLink = links[i].getAttribute('href');
      newLink = `/${franchiseToUse}${getLink}`;
      links[i].setAttribute('href', newLink);
    }
  }
}
```
## Call the function
when calling the function you'll have to declare two parameters:\
* `linkClass` is the class added to the `a` tag you're targeting, for example `<a class="brand__links">my link</a>`
* `franchises` which is going to be our global franchise variable `siteFranchises`
So it gets called like this:\

``` js
document.addEventListener('DOMContentLoaded',function() {
setBrandLink('.brand__links', siteFranchises); 
});
```


