# Dynamic Enhanched Icons for Used Details / New Cars
### CURRENTLY BEING TESTED 
**Plugin requires Babel to work on IE11 as it needs to be compiled.**


## What it does
This plugin was built in order to have a way to dynamically populate enhanched data with progress circles on used/new car details with an actual real percentage value that's not just a visual figure but it actually represents a real world percentage
 
## Requirements
Setup for the details pages requires you to get the literals for the values you need inside a script tag at the bottom of the page in order for them to be picked up in the global.js file after they've been rendered.\
like this:

**Used Car Details**
``` js
<script>
/* Enhanched data value storage -- do not delete*/
var co2Emissions, mpg, taxCosts, bhp;
co2Emissions = '<%= CO2.Text %>';
mpg = '<%= MPG.Text %>';
taxCosts = '<%= TaxCost.Text %>';
bhp = '<%= BHP.Text %>';
       
// properties arrays for the enhanched data 
var usedDetailsProperties = [
	{
	  name: 'Co2 Emissions', 
	  value: co2Emissions,
	  icon: 'icon-leaf',
	  maxValue: 300,
	  unit: 'g/km',
	  strokeColor: '#029852'
	},
	{
	  name: 'Mpg',
	  value: mpg,
	  icon: 'icon-gas-pump',
	  maxValue: 100,
	  unit: 'mpg',
	  strokeColor: '#86A6CC'
	},
	{
	  name: 'Tax Costs',
	  value: taxCosts,
	  icon: 'icon-road',
	  maxValue: 500,
	  unit: 'p/y',
	  strokeColor: '#E36538'
	},
	{
	  name: 'Engine Power',
	  value: bhp,
	  icon: 'icon-engine',
	  maxValue: 300,
	  unit: 'bhp',
	  strokeColor: '#CB2922'
	}
  ];

  document.addEventListener('DOMContentLoaded', function() {
    setCircleProgress('.data-items__container', ('col-sm-6', 'col-xl-3'), usedDetailsProperties);
  })
</script>
```

**New Car Details**
``` js
  <script>
    /* Enhanched data value storage -- do not delete*/
    var co2Emissions, mpg, taxCosts, bhp;

    co2Emissions = '<%=_cogCmsNewCarModel.DataCarbonEmissions%>';
    mpg = '<%=_cogCmsNewCarModel.DataMpg%>';
    topSpeed = '<%=_cogCmsNewCarModel.DataTopSpeed%>';
    bhp = '<%=_cogCmsNewCarModel.DataBhp%>';
           
    // properties arrays for the enhanched data 
    var newDetailsProperties = [
      {
        name: 'Co2 Emissions', 
        value: co2Emissions,
        icon: 'icon-leaf',
        maxValue: 300,
        unit: 'g/km',
        strokeColor: '#029852'
      },
      {
        name: 'Mpg',
        value: mpg,
        icon: 'icon-gas-pump',
        maxValue: 100,
        unit: 'mpg',
        strokeColor: '#86A6CC'
      },
      {
        name: 'Top Speed',
        value: topSpeed,
        icon: 'icon-tachometer-alt-fastest',
        maxValue: 180,
        unit: 'mph',
        strokeColor: 'yellow'
      },
      {
        name: 'Engine Power',
        value: bhp,
        icon: 'icon-engine',
        maxValue: 300,
        unit: 'bhp',
        strokeColor: '#CB2922'
      }
      ];
      document.addEventListener('DOMContentLoaded', function() {
        setCircleProgress('.data-items__container', ('col-xs-12','col-md-6'), newDetailsProperties);
      })
    </script>
```

The only difference between used and new is how the literals are defined in the templates.\

## How to set up properties 

It's self explanatory how this one needs to be laid out, the only things I would mention are `value` and `maxValue`

`value` is the variable you're passing through in the script tag mentioned at the top of this page, which is associated with vb literals coming from the used car details page.  
  
the object key `maxValue` is a real life comparison value depending on some variables.\
What's a high number for a car Co2 emissions on average? `300` seems like a high value for a car; These would be set up according to what the designer thinks is best based on the dealership and what cars they sell. For example if I'm looking at HR Owen I'd set up BHP `maxValue` to something like 700 or even 800.   

The plugin will make a calculation against the vehicle's value and spit out a percentage that will then fill the circle. 


The function `setCircleProgress` accepts three parameters, will go in depth about what they do below. \

## HTML Markup

You would only need to have these particular classes in order to populate the values in the right places: 

* `.enhanched-data__icon` element that will display the icon class you get from linearicons, example `icon-gas-pump`.
* `.data-name__text` element that will display the name of the data, example: `Mpg`
* `.data-value__text` element that will display the value of your data
* `.data-unit__text` element that will display the measuring unit of your data
* `.progress_value` the div that has the stroke dasharray css style applied to it
* `.circular-chart` the div of the svg element

**This is example code**
``` html
<div class="row my-5 data-items__container">
<div class="col-sm-6 col-xl-3 enhanched-data-items">
    <div class="row">
        <div class="col-6 align-self-center data__values">
            <div class="data-name">
                <p class="data-name__text"> 
                </p>
                <h3 class="data-value__text">
                </h3> <span class="data-unit__text"></span>
            </div>
        </div>
        <div class="col-auto align-self-center">
            <!-- progress circle-->
            <div class="circle-progress">
                <svg  viewBox="0 0 36 36" class="circular-chart">
                    <path class="circle-bg" d="M18 2.0845 a 15.9155 15.9155 0 0 1 0 31.831 a 15.9155 15.9155 0 0 1 0 -31.831" />
                    <path id="positive-circle" class="circle progress_value" stroke-dasharray="" d="M18 2.0845 a 15.9155 15.9155 0 0 1 0 31.831 a 15.9155 15.9155 0 0 1 0 -31.831" />
                    <text x="18" y="20.35" class="percentage percentage-count">
                        <span class="enhanched-data__icon icon"></span>
                    </text>
                 </svg>
            </div>
        </div>
    </div>
</div>
</div>
```
**You'll only need to hardcode one of these where you want them to display, the amount of items you'll get depends on how many you declare in the javascript object listed below.**

## The JavaScript Code
**Here's the code, I will break it down under this section to explain how it works**
``` js
// circle progress bars handler
const setCircleProgress = (outputDiv,bootstrapColumnValue,propertiesArray) => {
    let dataItemsContainer = document.querySelector(outputDiv);
    if(dataItemsContainer) {
    // create as many components as the properties
    // one already exists, create the rest
    let isComplete = 0;
    for(let x=0; x<(propertiesArray.length - 1);x++) {
      let circleElement = document.querySelector('.enhanched-data-items');
      let clonedElement = circleElement.cloneNode(true);
      dataItemsContainer.appendChild(clonedElement);
      isComplete++;
    }
    let circleElements;
    circleElements = document.querySelectorAll('.enhanched-data-items');
    // set a column class for each element 
    if(bootstrapColumnValue) {
      for(let z=0;z<circleElements.length;z++) {
      circleElements[z].classList.add(bootstrapColumnValue);
      }
    }
    // loop through all the circle elements
    if(isComplete===(propertiesArray.length-1)) {
    for(let i=0; i<circleElements.length; i++) {
    // loop through all the properties
        let dataIcon, dataName, dataValue, percentage,
          dataPercent, circularChart, dataUnit;
          dataIcon = circleElements[i].querySelector('.enhanched-data__icon');
          dataName = circleElements[i].querySelector('.data-name__text');
          dataValue = circleElements[i].querySelector('.data-value__text');
          dataUnit = circleElements[i].querySelector('.data-unit__text');
          dataPercent = circleElements[i].querySelector('.progress_value');
          circularChart = circleElements[i].querySelector('.circular-chart');
          // set values and circle percentage		
          dataValue.textContent = propertiesArray[i].value;
          dataIcon.classList.add(propertiesArray[i].icon);
          dataName.textContent = propertiesArray[i].name;
          dataUnit.textContent = propertiesArray[i].unit;
          percentage = ((parseInt(propertiesArray[i].value, 10)/propertiesArray[i].maxValue)*100).toFixed(); 
          dataPercent.style.strokeDasharray = `${percentage.toString()}, 100`;
          circularChart.style.stroke = propertiesArray[i].strokeColor;
          // set fallback value if data is missing 
          if(dataValue.textContent.length === 0) {
            dataValue.textContent = 'TBC';
            dataUnit.textContent = '';
            dataPercent.style.strokeDasharray = '0, 100';
          }
      }
    }
  }
}

```


## How to Call the function
``` js
      document.addEventListener('DOMContentLoaded', function() {
        setCircleProgress('.data-items__container', ('col-xs-12','col-md-6'), newDetailsProperties);
      })
```
The function accepts three parameters (the output div, bootstrap column classes, the array to get the properties from)
1. Parameter 1: `data-items__container` would be the container div that wraps your hardcoded empty component
2. Parameter 2: (**optional**) would be the bootstrap class of the single element (in order to adapt to different layouts) depending where it's needed.
3. Parameter 3: the array to get the properties from, which in this example can be either `newDetailsProperties` or `usedDetailsProperties`



working example of this can be found on any used car details page on Telford: 
*[Telford Group](http://ttg18344.dev.cogplatform.co.uk/skoda/used-car-details/used-citroen-c1-10-i-connexion-3dr-hatchback-black-manual-petrol/id-94754194108/)


