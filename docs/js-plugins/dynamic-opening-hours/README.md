# Overview

**THIS PLUGIN IS MEANT TO BE USED ON LEGACY SITES ONLY**

This plugin was made to give Client Services a way of controlling Christmas/Easter Opening hours -- Or any other kind of special announcement on legacy websites in an attempt to limit the amount of ads jobs during those periods of time.

## Documentation for Design

### How to Set up 
Depending on where you need this to display I would generally recommend putting this in a file that's on every page like a header or a js-head file. <br><br>
This is the recommended file structure:<br>

Have an `announcements.aspx` include with all of the types of announcements the site needs, for example `christmas, easter and general` <br><br>

Inside the `announcements.aspx` file you're going to have this:

``` html
<link href="/core/css/assets/fonts/mbblueskyicons/style.css" rel="stylesheet" media="all">
<!--#include file="/inc/components/announcements/christmas.aspx"-->
<!--#include file="/inc/components/announcements/easter.aspx"-->
<!--#include file="/inc/components/announcements/general.aspx"-->

<!-- bluesky cdn script link-->
<script src="https://www.blueskyinteractive.co.uk/content-delivery/scripts/announcements/main.js"></script>
```

And either have a style tag inside of it or an external css for it, I've used a style tag in my case.
::: warning
This is the CSS I've used for a specific case, know that you can always target .announcement-light for the light theme and .announcement-dark for the dark theme
:::
**So the file will look like this**:
```html
<link href="/core/css/assets/fonts/mbblueskyicons/style.css" rel="stylesheet" media="all">
<!--#include file="/inc/components/announcements/christmas.aspx"-->
<!--#include file="/inc/components/announcements/easter.aspx"-->
<!--#include file="/inc/components/announcements/general.aspx"-->

<!-- bluesky cdn script link-->
<script src="https://www.blueskyinteractive.co.uk/content-delivery/scripts/announcements/main.js"></script>
<style>
    /* declaration of fallback styles for legacy websites */
    .d-none {
        display: none;
    }
    .d-block {
        display: block;
    }
    /* plugin appearance */
    .announcement-container {
        max-width: 100%;
        padding: 15px;
        text-align: center;
    }
    .announcement-light {
        color: #242424;
    }
    .announcement-dark {
        color: #fff;
    }
    /* absolute positioning for not pushing mode */
    .announcement-absolute {
        position: absolute;
        z-index: 99999;
        top: 0;
        width: 100%;
        left: 0;
    }
    /* relative container for clearance */
    .announcement-relative__container {
        position: relative;
    }
    /* dark table style */
    .announcement-dark table {
        background: transparent;
    }
    .announcement-dark tr {
        background: transparent;
    }
    .announcement-dark table tr:nth-child(2n+1) {
        background: rgba(40, 40, 40, .5);
    }
    .announcement-dark table td {
        border-bottom: 0px;
        padding: 7px 5px;
    }
    .announcement-dark table th {
        padding: 7px 5px;
        border-bottom: none;
    }
    /* light table style */
    .announcement-light table {
        background: transparent;
    }
    .announcement-light tr {
        background: transparent;
    }
    .announcement-light table tr:nth-child(2n+1) {
        background: rgba(40, 40, 40, .2);
    }
    .announcement-light table td {
        border-bottom: 0px;
        padding: 7px 5px;
    }
    .announcement-light table th {
        padding: 7px 5px;
        border-bottom: none;
    }
    /* end light table style */
    .announcement-content {
        transition: max-height .27s ease-in-out;
    }
    .announcement-content.closed {
        max-height: 0;
        overflow: hidden;
    }
    .announcement-content.open {
        max-height: 760px;
        padding: 20px 0;
    }
    .announcement-container .icon {
        margin: 0px 10px;
    }
    .announcement-toggle {
        cursor: pointer;
    }
</style>
```

### Do I need Linearicons?
**Yes you do**, Unless the website you're setting this up in already has it; <br><br>
The plugin is set to use Linearicons so you're going to have to check if it's included in the site you're working it or not.
If it isn't you can always get it from any new website (blueskyinteractive.co.uk) inside this folder:
``` 
/core/css/assets/fonts/mbblueskyicons
```

<br><br>
::: warning
I don't recommend editing this CSS directly, treat it as a plugin css, if you have to make a change overwrite it 
:::

<br><br>

This is all you need for the main `announcements.aspx` file, let's now talk about the specific announcement types. <br><br>

For the actual opening hours component this is what you would have, let's pick `christmas.aspx` as an example:
```html
<div class="announcement-container christmas d-none"
     data-is-enabled="false"
     data-close-text="Close"
     data-open-text="Open"
     data-open-icon="icon-chevron-down2"
     data-closed-icon="icon-chevron-up2"
     data-background="#eeeeee"
     data-bg-light-or-dark="light"
     open-by-default="false"
     data-does-push="true"
     >
    <div class="announcement-content closed">
    <!-- have a specific ID like opening-christmas-hours in order to not get into dup-id 
         issues with Surreal CMS -->
        <div class="editable" id="opening-christmas-hours">
       <!-- your html content here
            Have it empty OR commented out if the panel is disabled
        -->
    </div>
    </div>
    <div class="announcement-toggle"></div>
</div>
```

The data attributes are self explanatory, I would only mention a couple of things:

* `data-is-enabled` enables the specific opening hours panel (it essentially removes the d-none class) **only one should be active at a time!**

* `data-close-text` is the text that will be displayed when the panel is open, like `Close this panel`

* `data-open-text` is the text that will be displayed when the panel is closed, like `Open this panel` or `Click here for Christmas Opening Hours`

* `data-open-icon` references the two icons before and after the open text, which will be the classname from linearicons

* `data-closed-icon` references the two icons before and after the close text, which will be the classname from linearicons

* `data-background` is just the background colour of the panel, it can be hex or anything accepted by css rules

* `data-bg-light-or-dark` accepts a string, either `light` or `dark` and it's essentially deciding the colour of the fonts inside the panel between black and white, if you choose `dark` the font will be **white**, if you choose `light` the font will be **dark**.

* `open-by-default` accepts `true` or `false` and its property determines if the panel is already open when you land on the page or not

* `data-does-push` accepts `true` or `false` and its property determines if opening the panel pushes the page down or stays absolutely positioned with a higher z-index. 

::: danger
Notice the **id="opening-christmas-hours"** , I highly recommend having a specific ID for editable regions otherwise you might run into duplicate ID problems with Surreal CMS 
:::

**That's it!**

*Remember to enable the pages you created (christmas,easter,general or other) in Surreal CMS for editing so then CS can go in and edit them.*




## Documentation for Client Services

The opening hours plugin requires Design Setup so an ads job will need to be put in place with a link to this Documentation if you want to have it on one of the websites you manage.

Chances are that if you're reading this a designer sent you the link and it's already set up on your website, if that's the case then **LET'S GET STARTEEEED** :tada: :100: :100: *(please don't @ me for using emojis in the documentation)*

### Enabling the opening hours panel
First go to `https://edit-content.com/` and get to the website you need to edit <br>


If the set up was done correctly and the pages are enabled for editing you should be able to look for `Christmas` `Easter`  `General` or `Announcement` and find the page you need to edit

Open the file and go to `Edit Source` <br>

After opening the file you'll probably see something like this:

```html
<div class="announcement-container christmas d-none"
     data-is-enabled="false"
     data-close-text="Close"
     data-open-text="Open"
     data-open-icon="icon-chevron-down2"
     data-closed-icon="icon-chevron-up2"
     data-background="#eeeeee"
     data-bg-light-or-dark="light"
     open-by-default="false"
     data-does-push="true"
     >
    <div class="announcement-content closed">
        <div class="editable" id="opening-christmas-hours">
        <!-- OPENING HOURS HERE
        OR ANY OTHER KIND OF ANNOUNCEMENT
         -->
    </div>
    </div>
    <div class="announcement-toggle"></div>
</div>
```

All you care about in this file are the `data-` attributes which I'm gonna list below and the html inside the `editable` class. 

### The Attributes
<br>

* `data-is-enabled` determines if the opening hours panel is showing on the website or not, change between `true` or `false` to make it display or not display.

::: danger
Only one panel should be active at any given time, please check the other ones are disabled when enabling one.
:::

* `data-close-text` is what you want to display on the panel when the panel is **open** like for example **Close Opening Hours**; Basically the text that when clicked toggles the panel from open to closed.

* `data-open-text` is the text you want to display on the panel when it's **closed** so like **Click here for Christmas Opening Hours!!! Seriously... CLICK HERE FOR OPENING HOURS DUDE**

#### Open and Closed Icons `data-open-icon` and `data-closed-icon`

**Icon Reference**:
[Bluesky Icon Reference](http://boilerplatedev.dev.cogplatform.co.uk/core/css/assets/fonts/mbblueskyicons/demo.html)

So if you open the Bluesky Icon Reference link you'll see a page with all the icons we use on our websites, here you can choose which icons you want to use to wrap Open and Close text with

let's say you wanna use the first icon on the page, which is `icon-wallet`

You would have to copy the icon name `icon-wallet` and paste it in either `data-open-icon` or `data-close-icon`

so as an example:

```html 
data-open-icon="icon-wallet"
```

Will end up looking like this in the front end: <br><br>
![Open Close Icons](/img/opening-hours/01.jpg)

*Not sure who'd want two wallets surrounding text but oh well*

And the same can be done for `data-close-icon`, it gives you the option of having different icons for opening and closing, like if you want arrows down for opening and arrows up for closing kinda thing.

* `data-background` is a property that lets you change the background colour of the panel, you'll need a specific colour value for this and you could grab it from [Here](https://givemecolors.netlify.com/), you can just click on the color you want and it will be ready to be pasted wherever you need it.

* `data-bg-light-or-dark` accepts two values: `light` or `dark`, if it's `light` the font color of the panel will be **black** and if it's `dark` the font color of the panel will be **white**

* `data-open-by-default` accepts `true` or `false` and it determines if the panel is going to be open when you land on the page or not.

* `data-does-push` accepts `true` or `false` and it determines if the panel pushes the page down when opened or it's independent from the rest of the page.

**That's IT!**
You can now enable/disable and customise opening hours without having to submit two ads jobs for it.

If you have any problems feel free to contact any designer on Slack and we will be able to help configuring/fixing it.

