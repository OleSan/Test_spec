# Specification for HTML based adverts

This is the specification for the HTML creatives on Schibsted sites that use AdTech as ad delivery system. 

## 1. Dimensions

### Height and width
* Link to formats page (needs to be edited)

### Size
* Max 100 kB in compressed state (all files compressed/zipped together).
* You can use jQuery from [Googles CDN service](https://developers.google.com/speed/libraries/devguide#jquery) without its size counting against the total size of the ad.
* Resources loaded after a user interaction does not count against the total size of the ad.

Note: The size limitation is on the initial load. You can lazy load additional content, but for the first rendering this is the limit.

## 2. Limitations

* Viewport can not be set to device width within the ad 
 * ~~`<meta name="viewport" content="width=devicewidth">`~~
* The [Geo Location APIs](https://github.com/inma-no/advertsspec/blob/master/spec/geoapi.md) or other "HTML5"-APIs that require user confirmation, can only be used after a user interaction.
* The HTML-file delivered should just be _one file_ with all CSS required for the ad inline in the HTML. The code should not be mimimized for better support in AdTech. 
* JavaScript has to be in a seperate .js file, never inline in the HTML file. Add the JS file right before the ending body tag. 
* [Maximum of two HTTP requests to JavaScript libraries (at least one to CDN).](https://github.com/inma-no/advertsspec/blob/master/spec/maximumhttprequests.md)
* Animation _prior to a user interaction_ must be written using [CSS3 Transitions, Transforms and/or Animation](https://github.com/inma-no/advertsspec/blob/master/spec/cssforanimations.md) or with the Greensock Library hosted from Cloudfare (legge til lenken). 
 * Other [JavaScript animations are forbidden before an user interaction](https://github.com/inma-no/advertsspec/blob/master/spec/jsanimations.md).
* References to resources must be absolute and start with http:// or https:// , not only //. Because of limitations in our delivery system.
* Only use `event.preventDefault` or `event.stopPropagation` for touch-events moving horizontally +/- 30°.
* Do not use `touchstart` as an alias for `click`. Because `touchstart` is fired when the user is scrolling.
* Only trigger audio or video resources using `touch` or `mouse` events.

## 3. Click counter
You can use both anchor tags and JavaScript for click handling. If you are going to use JavaScript for this, you'll need to add a little snippet of code to get click counted in AdTech. 

In HTML file:
```
<script>var clickCounter = '_ADCLICK_';</script>
```

In the JS-file:
```
// where you define the landing page URL in your JS file. 
var landingpage = clickCounter + 'www.example.com';
```

Note: [Paid links are required by Google](https://support.google.com/webmasters/answer/96569?hl=en) to have the attribute `rel="nofollow"`

## 4. Default styling of the creative

CSS rules can only be set to classes or IDs. You can not set rules directly to elements like span or div.

The div contaner element should only have the following styling:

    display: block; /* browser default */
    position: static; /* browser default */
    width: XXXpx; //change to format width
    height: XXXpx; //change to format height
    cursor: pointer;

Additional styling must be done on a new div/element within that container (ex Banner as container in the sample below).

    <div id="Banner" onclick="window.open('http://www.url.no','new_window');" 
    	style="display:block;width:100%;height: 250px;">
        <div style="position:relative;"></div>
    </div>  

Read additional [styling tips](https://github.com/inma-no/advertsspec/blob/master/spec/stylingingtips.md).

## 5. JavaScript libaries
The following JavaScript libraries are allowed:

* The two latest minimized jQuery verisons from Google CDN services (add link). 
 * jQuery can _not_ be used for animation
* Greensock library for animations hosted from Cloudfare (add link)

## 6. Creative delivery
The creative should be delivered in a zip file without folders. Name the HTML file index.html. The file names can only be written with 7bit ASCII characters, so no characters like @ or % etc. The HTML file should not have minimized code. This to ensure best possible support for AdTech.

## 7. Third party code
* Must be sent as JavaScript code


