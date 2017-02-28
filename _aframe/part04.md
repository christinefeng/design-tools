---
layout: page
title: 'Lesson 4: Creating a cursor'
---
> Download [Argon4](http://argonjs.io/argon-app) and the [Tutorial Source Code](https://github.com/argonjs/design-aids/tree/gh-pages/code). <br> This tutorial uses the *cursor* and *resources* directories.<br> **[Demo in Argon4](https://github.com/argonjs/design-aids/tree/gh-pages/code/cursor/)**


In an HTML page, the user typically selects an item by clicking on it with the cursor. You can also add a cursor to the 3D world of Argon-aframe. When you add the cursor to the camera entity, it will appear at a place you specify relative to the center of the screen. This cursor shape will move as the user moves the phone. (If the user is using a Google cardboard or similar device, then turning her head will move the cursor.) The cursor can be activated by a click by the user or simply by keeping it steady over an object for a length of time (the fuse time). Look at the example below:

```
	<body>
    <ar-scene>
      <a-entity id="helloworld" position="0 -1 -8">
        <a-sphere position="0 1.25 -1" cursor-listener radius="1.25" color="#EF2D5E" ></a-sphere>
      </a-entity>
      <ar-camera>
        <a-entity id="myCursor" cursor="fuse:true; fuse-timeout: 1000"
                    position="0 0 -0.1"
                    geometry="primitive:ring; radiusInner: 0.001; radiusOuter: 0.0015"
                    material="color: #2E3A87; opacity:0.3;">          
        </a-entity>
      </ar-camera>
    </ar-scene>
```  
 
This simple `<ar-scene>` consists of two things: a sphere and the ar-camera. The camera has a cursor entity attached to it. The cursor entity is to set "fuse", which means that it will select an object by just keeping the cursor over the object, in this case for 1000 miliseconds = 1 sec. The other components of the cursor entity define how it looks: a ring of a certain color and 30% opacity.  (You can define the shape and color of your own cursor, if you don't like the ring.)
 
At this point, the cursor is defined, but it doesn't have any effect on the objects in the scene beyond just selection. In order to make the cursor have an effect, we need to add Javascript code that "listens" for the cursor when it selects an object. We do this by registering a new "component". This particular component is called `cursor-listener`. It is coded in a script tag and "registered" with A-Frame (see below). Once registered, the component can be attached to any appropriate object - in this case the sphere - as you can see in this line from the HTML above:  

```
<a-sphere position="0 1.25 -1" cursor-listener radius="1.25" color="#EF2D5E" ></a-sphere>
```

The sphere will react to the click-event, the mouseenter-event, and the mouseleave-event. These events make the sphere reactive to the cursor. Here is the component that makes this possible: 

```  
    <script>
      AFRAME.registerComponent('cursor-listener', {
        init: function () {
          this.el.addEventListener('click', function (evt) {
            console.log('I was clicked at: ', evt.detail.intersection.point);
          });
          this.el.addEventListener('mouseenter', function (evt) {
            this.setAttribute('material', 'opacity', 0.5);
          });
          this.el.addEventListener('mouseleave', function (evt) {
            this.setAttribute('material', 'opacity', 1.0);
          });
        }
      });
	</script>
	</body>
```

The function defined in the component is executed once when the component is initialized for a particular object. It adds three eventListeners (for eventListeners, see [lesson 5](http://argonjs.io/design-tools/aframe/part05/)). One event fires when the user clicks on the particular object. When that happens, the program prints the message 'I was clicked at: ' along with the point where the cursor indicated contact with the object. The other two events fire when the cursor starts hovering over the object or moves away from the object. Hovering over the object will cause it to become translucent (opacity = .5). Moving away will cause the object to return to its full opacity. 

You can put any Javascript code you want into these events. You could make audio play when the user clicks on the sphere, or you could make the sphere rotate, or display text on the screen. This is how you introduce interactivity into the scene. 

You can learn more about components in [lesson 12](http://argonjs.io/design-tools/aframe/part12/). They are a key feature of working with A-Frame (and Argon-aframe).
