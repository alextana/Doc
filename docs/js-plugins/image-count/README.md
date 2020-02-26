# Image Counter for Slick Slider
**Plugin requires Babel to work on IE11 as it needs to be compiled.**
**the Es5 version will be included at the bottom of the page** 

## Plugin 
``` js
// shows image index on slick gallery 
const imageIndexForSlider = (outputDiv, galleryDiv, sliderImages) => {
	let imageIndexOutput, galleryImages, elementsToCheck, slickNext, slickPrev, usedGallery;
	imageIndexOutput = document.querySelector(outputDiv);
	usedGallery = document.querySelector(galleryDiv);
	if (imageIndexOutput) {
		// add timeout to let slider load
		setTimeout(function () {
			galleryImages = document.querySelectorAll(sliderImages);
			slickNext = usedGallery.querySelector('.slick-next');
			slickPrev = usedGallery.querySelector('.slick-prev');
			elementsToCheck = [slickPrev, slickNext];
			// loop that checks the index of the image that's active
			let imageLoop = () => {
				for (var i = 0; i < galleryImages.length; i++) {
					if (galleryImages[i].parentElement.parentElement.classList.contains('slick-active')) {
						imageIndexOutput.textContent = `Image ${i+1} of`
					}
				};
			}
			// loop to listen to clicks on both slick prev and slick next
			for (var x = 0; x < elementsToCheck.length; x++) {
				// act on load
				if (galleryImages[i].parentElement.parentElement.classList.contains('slick-active')) {
					imageIndexOutput.textContent = `Image ${i+1} of`
				}
				// act when clicking on slick-next or slick-prev
				elementsToCheck[x].addEventListener('click', function () {
					imageLoop();
				});
			}
			// act on touch
			usedGallery.addEventListener('touchend', function () {
				imageLoop();
			});
		}, 500);
	}
}
```
The script loops through the gallery and shows the index in the front end. 

## How to use
* Copy and paste the plugin code in your used-details.js file (or wherever you need to display the image index)
* Call the function wherever it's needed

## How to properly call the function
This function accepts three parameters, which are class names
Here is an example: 

``` js
imageIndexForSlider('firstclass', 'secondclass', 'thirdclass');
```

* First Parameter: Output div (which is the div where you're going to display "Image [index] of")
* Second Parameter: Gallery div (which is the gallery container, basically the parent element to slick-prev and slick-next)
* Third Parameter: Slider Images (assign the same class to the slider images and this will be your third parameter)

## Example of a function call on a real website 

Vanilla JS document ready statement with the included parameters
``` js
document.addEventListener('DOMContentLoaded', function () {
	imageIndexForSlider('.image__index', '.used__gallery', '.slider__image');
});
```

## Working example on a real website 
* [Telford Group](http://ttg18344.dev.cogplatform.co.uk/skoda/)


## Compiled ES5 Version if you don't use Babel
``` js
var imageIndexForSlider = function imageIndexForSlider(outputDiv, galleryDiv, sliderImages) {
  var imageIndexOutput, galleryImages, elementsToCheck, slickNext, slickPrev, usedGallery;
  imageIndexOutput = document.querySelector(outputDiv);
  usedGallery = document.querySelector(galleryDiv);

  if (imageIndexOutput) {
    // add timeout to let slider load
    setTimeout(function () {
      galleryImages = document.querySelectorAll(sliderImages);
      slickNext = usedGallery.querySelector('.slick-next');
      slickPrev = usedGallery.querySelector('.slick-prev');
      elementsToCheck = [slickPrev, slickNext]; // loop that checks the index of the image that's active

      var imageLoop = function imageLoop() {
        for (var i = 0; i < galleryImages.length; i++) {
          if (galleryImages[i].parentElement.parentElement.classList.contains('slick-active')) {
            imageIndexOutput.textContent = "Image ".concat(i + 1, " of");
          }
        };
      }; // loop to listen to clicks on both slick prev and slick next
      for (var x = 0; x < elementsToCheck.length; x++) {
        // act on load
        if (galleryImages[i].parentElement.parentElement.classList.contains('slick-active')) {
          imageIndexOutput.textContent = "Image ".concat(i + 1, " of");
        } // act when clicking on slick-next or slick-prev
        elementsToCheck[x].addEventListener('click', function () {
          imageLoop();
        });
      } // act on touch
      usedGallery.addEventListener('touchend', function () {
        imageLoop();
      });
    }, 500);
  }
};
```


