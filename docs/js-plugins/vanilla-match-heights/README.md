# Vanilla JS Match Heights

## Plugin: 
``` js
// match heights vanilla 
const matchHeights = (matchClass) => {
    const sameHeight = document.querySelectorAll(matchClass);
    let heightsArray = [];
    if(sameHeight){
        for(var i=0;i<sameHeight.length;i++) {
            // get height of the element and push it to an array
            heightsArray.push(sameHeight[i].clientHeight);
        }
    // check biggest number
    let biggestNumber = Math.max(...heightsArray);
    // loop again to set heights
    for(var x=0;x<sameHeight.length;x++) {
        sameHeight[x].style.height = `${biggestNumber}px`;
    }
 }
}
```
## Why it can be useful?
Takes away one more plugin dependant on jQuery 

## How to use? 
* Paste this plugin in any js file (ideally global.js)
* Call the plugin wherever it's needed like this:

If you're using babel
``` js
document.addEventListener('DOMContentLoaded', () => {
    matchHeights('.yourclass');
})
```

If you're not using babel
``` js
document.addEventListener('DOMContentLoaded', function(){
    matchHeights('.yourclass');
})
```

