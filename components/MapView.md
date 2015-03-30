## Props 

**legalLabelInsets** {top: number, left: number, bottom: number, right: number} 

Insets for the map's legal label, originally at bottom left of the map. See `EdgeInsetsPropType.js` for more information.

**maxDelta** number 

Maximum size of area that can be displayed.

**minDelta** number 

Minimum size of area that can be displayed.

**onRegionChange** function 

Callback that is called continuously when the user is dragging the map.

**onRegionChangeComplete** function 

Callback that is called once, when the user is done moving the map.

**pitchEnabled** bool 

When this property is set to `true` and a valid camera is associated with the map, the camera’s pitch angle is used to tilt the plane of the map. When this property is set to `false`, the camera’s pitch angle is ignored and the map is always displayed as if the user is looking straight down onto it.

region {latitude: number, longitude: number, latitudeDelta: number, longitudeDelta: number} 

The region to be displayed by the map.

The region is defined by the center coordinates and the span of coordinates to display.

**rotateEnabled** bool 

When this property is set to `true` and a valid camera is associated with the map, the camera’s heading angle is used to rotate the plane of the map around its center point. When this property is set to `false`, the camera’s heading angle is ignored and the map is always oriented so that true north is situated at the top of the map view

**scrollEnabled** bool 

If false the user won't be able to change the map region being displayed. Default value is true.

**showsUserLocation** bool 

If `true` the app will ask for the user's location and focus on it. Default value is false.

**NOTE**: You need to add NSLocationWhenInUseUsageDescription key in Info.plist to enable geolocation, otherwise it is going to fail silently!

**style** [View#style](http://facebook.github.io/react-native/docs/view.html#style)

Used to style and layout the MapView. See `StyleSheet.js` and `ViewStylePropTypes.js` for more info.

**zoomEnabled** bool 

If `false` the user won't be able to pinch/zoom the map. Default `value` is true.