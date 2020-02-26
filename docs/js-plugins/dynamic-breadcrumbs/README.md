
# Dynamic Breadcrumbs
**Plugin requires Babel to work on IE11 as it needs to be compiled.**

# The Plugin
## How to Use
* Copy and paste the plugin code in your global.js file (or wherever you need it)
* Copy and paste the SCSS in any of your scss files, I've created a breadcrumbs.scss file and referenced in my homepage for clarity
* Copy and paste the HTML wherever you need the breadcrumbs to display
* Call the function wherever you need it


## HTML

``` html
      <!-- breadcrumbs -->
      <div class="container-fluid px-0 py-2 gray__bg">
        <div class="container container__large">
          <div class="row justify-content-left">
            <div class="col-12 your__class">
            </div>
          </div>
        </div>
      </div>
      <!-- end breadcrumbs -->
```
Where `your__class` is the class for the output

## SCSS
``` scss
.link__class {
    text-transform: capitalize;
    font-weight: bold;
    &:after {
        content: '/';
        margin: 0px 10px;
        pointer-events: none;
        color: #242424;
    }
    &:first-child() {
        &:before {
            content: "";
            font-family: "mbblueskyicons";
            color: #242424;
            margin-right: 10px;
        }
    }
    &:last-child() {
        pointer-events: none;
        color: #242424;
        &:after {
            content:''!important;
        }
    }
}
```
the SCSS file handles the separators "/" and the home icon for the first element

## JavaScript
``` js
// generate dynamic breadcrumbs
const createBreadcrumbs = (breadcrumbContainer, homeText, breadcrumbCustomLink) => {
  let breadcrumb;
  breadcrumb = document.querySelector(breadcrumbContainer);
  if(breadcrumb) {
    // get location pathname to get all the parameters
    let pathname = location.pathname.split('/');
    // create an empty array to store the pathnames without the empty values
    // created by the split
    let cleanArray = [];
    for(var i=0; i<pathname.length;i++) {
      // push the values into the new array if they're not empty
      if(pathname[i] !== '') {
        cleanArray.push(pathname[i]);
      }
      // this is to check when the loop is complete and the array
      // is ready to be worked with
      if(i === (pathname.length - 1)) {
        // create the first link (home, group home etc.)
        let link = document.createElement('a');
        link.href = '/';
        link.classList.add(breadcrumbCustomLink);
        link.innerHTML = homeText;
        breadcrumb.appendChild(link);
        // create the dynamic links
        for(var x=0;x<cleanArray.length;x++) {
            let breadcrumbLink = document.createElement('a'); 
            // splits href at the current instance and add its own instance to it
            // basically get everything that's before itself and adds itself at the end
            let currentLink = location.href.split(cleanArray[x])[0] + cleanArray[x] + '/';
            breadcrumbLink.href = currentLink;
            // sanitise path for html display (remove hyphens)
            let sanitisedPath = cleanArray[x].replace(/-/g, ' ');
            breadcrumbLink.innerHTML = ` ${sanitisedPath}`;
            breadcrumbLink.classList.add(breadcrumbCustomLink);
            breadcrumb.appendChild(breadcrumbLink)
        }
      }
    }
  }
}
```

## How to call the function
``` js
document.addEventListener('DOMContentLoaded',function() {
createBreadcrumbs('.your__class', 'Group Home', '.link__class');
});
```
The function accepts three parameters:
* `.your__link` is the output div (where should the breadcrumbs output to)
* `'Group Home'` is the string you want to display for the first element of the breadcrumbs
* `.link__class` is a class that's added to all the dynamically generated links, which then is referenced in the SCSS for the styling.

## Example on a real site
[Telford Skoda](http://ttg18344.dev.cogplatform.co.uk/skoda/offers/)


