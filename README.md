# Insidemaps FloorPlan API 

![Splash](https://github.com/insidemaps-org/api-demo/blob/gh-pages/docs/splash.png)


## Try it now

[**Run Home Automation Demo**](http://insidemaps-org.github.io/api-demo/) - Runs directly from this page through http://insidemaps-org.github.io/api-demo/

## Embedding Interactive Floor Plans into Your Applications

InsideMaps:Embed gives users the ability to view and manage their entire connected house using a floor plan or 3D model interface of the home. This natural user experience enhances the "glance-ability" of the connected home, helping people more easily monitor all of their devices and platforms.

InsideMaps is the easiest and most affordable away to create an accurate floor plan and 3D model using just images captured by handheld cameras.

InsideMaps Embed for home automation is the same product used by real estate agents and furniture retailers to help users feel as if they were actually in the home.  

Every element of the floor plan is addressable to allow home automation solutions to easily associate devices and alerts to any particular location of the home making device activation, control and notifications intuitive and natural to understand.

Example use cases: 
1. If a "connected light" is turned on, the corresponding device in the floor plan turns orange.  Optionally, for easier readability, the whole room in which the device is located turns orange.
2. If a "connected motion detector" sends an alert, the corresponding device in the floor plan turns red.  Optionally, for easier readability, the whole room in which the device is located turns red.

## How it works:

We have made a series of API's available that allows an web or app developer to embed an insidemaps map into their application. 

InsideMaps floor plans and 3D models are comprised of walls, floors, doors, windows, furniture, etc. that are semantically equivalent to their real world counterparts.  

InsideMaps has a concept of smart-furniture and home automation devices is simply a class of objects that have attribute information and state variables that can be assigned to real world objects.

## API Overview

### Embedding a floor plan

To embed an insidemaps floor plan, initialize the insidemaps API by including the floor plan API in your JavaScript code.
```
<script type='application/javascript' src='https://www.insidemaps.com/apis/build/fpapi.min.js'></script>
```
Next, in your HTML, create a named ``div`` element where you want to embed the Insidemaps floor plan. Your application controls the size and position of the embedded floor plan in your page by controlling the size of this ``div``.

### Initialization

The next step will be to initialize the Insidemaps' API in your application's Javascript code. Create an instance of ``Insidemaps.FloorPlan`` class and call the ``init`` function and supply the following parameters:
* ``domTargetId`` - the name of the div element where you want to embed the map
* ``model`` - the id of the Insidemaps floor plan that you want to embed
* ``originURL`` - the url of your page
* ``params`` - you will want to set the ``autoPlay`` parameter to true if you want the floor plan to start loading directly without user interaction
```
	var floorPlan = new InsideMaps.FloorPlan();
	floorPlan.init( {
		domTargetId: 'insidemaps-floor-plan',
		model: 'WzqsJlqXY7',
		originUrl: location.protocol + '//' + location.host,
		apiKey: undefined, // Allow insidemaps to control/track which app is using our API.
		params: {
			autoPlay: true
		}
	})
	.then( function( data ) {
		initHomeAutomationObjects();
		floorPlan.addHAObjectEventListener( homeAutomationEventListener );
	});
```
Note: you will get a callback when the API is initialized (or you can use the returned [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) as illustrated in this example). At this point the floor plan is loaded and you can start interacting with the floor plan. Our example application takes the opportunity to register an event listener for Home Automation Object events, which means that when the user interacts with home automation objects in the floor plan, the application will get notifications through the supplied callback.

## Interacting with the Floorplan

A number of methods are available allowing your application to interact with objects in the embedded floor plan.

| Method | Description |
| ---- | ---- |
| **getHomeAutomationObjects** | Returns a list of all home automation objects in the floor plan |
| **notifyHAObject** |  Sends a notification to a home automation object |
| **getRooms** |  Returns an array of rooms that are a part of the floor plan |
| **getCurrentFloorName** |  Returns the name of the floor currently being viewed |
| **getFurniture** |  Returns an array of furniture objects placed on the floor plan |
| **addHAObjectEventListener** |  Registers a callback function that'll get called once home automation object has changed |

 
## FAQ
* Q: How do I edit the map and add home automation objects to the map?
* A: The editor for adding new furinture pieces to a model is available on the demos on insidemaps.com. The editor for adding home automation objects is not 100% ready but will work similary.
* Q: Can I get an InsideMaps 3D model and floor plan of my own house?
* A: Yes. Use the contact form on insidemaps.com to schedule a scan.
* Q: What home automation objects do InsideMaps support?
* A: InsideMaps support all types of home automation objects through the concept of 'smart furniture'. The demo demonstrates light switches, but any objects can be added to the smart furniture library.
* Q: What type of embedding is supported? Can I embedd a simpler map?
* A: The demo demonstrates WebGL rendering. There will also support for a pure html5 canvas rendering. That can target smaller devices and screens. In addition there will be a static image server endpoint to render a static image from a floorplan. This is good for email notifications and wearables like iWatch.
* Q: Besides home automation objects can I also setup interation with other elements?
* A: Yes. There will APIs to interact with rooms, walls and other structural elements to set highlight and react to events. As an example you can turn a room red if a motion sensor indicates activity.


## API Reference

Reference documentation for the Insidemaps FloorPlan API is available on the Insidemaps website at [insidemaps.com/apis/docs](https://www.insidemaps.com/apis/docs)

