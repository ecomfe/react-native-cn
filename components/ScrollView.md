Component that wraps platform ScrollView while providing integration with touch locking "responder" system.

Doesn't yet support other contained responders from blocking this scroll view from becoming the responder.

## Props 

**alwaysBounceHorizontal** bool 

When true, the scroll view bounces horizontally when it reaches the end even if the content is smaller than the scroll view itself. The default value is true when `horizontal={true}` and false otherwise.

**alwaysBounceVertical** bool 

When true, the scroll view bounces vertically when it reaches the end even if the content is smaller than the scroll view itself. The default value is false when `horizontal={true}` and true otherwise.

**automaticallyAdjustContentInsets** bool 

**centerContent** bool 

When true, the scroll view automatically centers the content when the content is smaller than the scroll view bounds; when the content is larger than the scroll view, this property has no effect. The default value is false.

**contentContainerStyle** StyleSheetPropType(ViewStylePropTypes) 

These styles will be applied to the scroll view content container which wraps all of the child views. Example:

return ( <ScrollView contentContainerStyle={styles.contentContainer}> </ScrollView> ); ... var styles = StyleSheet.create({ contentContainer: { paddingVertical: 20 } });

**contentInset** {top: number, left: number, bottom: number, right: number} 

**contentOffset** PointPropType 

**decelerationRate** number 

A floating-point number that determines how quickly the scroll view decelerates after the user lifts their finger. Reasonable choices include - Normal: 0.998 (the default) - Fast: 0.9

**horizontal** bool 

When true, the scroll view's children are arranged horizontally in a row instead of vertically in a column. The default value is false.

**keyboardDismissMode** enum("none", 'interactive', 'onDrag') 

Determines whether the keyboard gets dismissed in response to a drag. - 'none' (the default), drags do not dismiss the keyboard. - 'onDrag', the keyboard is dismissed when a drag begins. - 'interactive', the keyboard is dismissed interactively with the drag and moves in synchrony with the touch; dragging upwards cancels the dismissal.

**keyboardShouldPersistTaps** bool 

When false, tapping outside of the focused text input when the keyboard is up dismisses the keyboard. When true, the scroll view will not catch taps, and the keyboard will not dismiss automatically. The default value is false.

**maximumZoomScale** number 

The maximum allowed zoom scale. The default value is 1.0.

**minimumZoomScale** number 

The minimum allowed zoom scale. The default value is 1.0.

**onScroll** function 

**onScrollAnimationEnd** function 

**pagingEnabled** bool 

When true, the scroll view stops on multiples of the scroll view's size when scrolling. This can be used for horizontal pagination. The default value is false.

**removeClippedSubviews** bool 

Experimental: When true, offscreen child views (whose `overflow` value is `hidden`) are removed from their native backing superview when offscreen. This canimprove scrolling performance on long lists. The default value is false.

**scrollEnabled** bool 

scrollIndicatorInsets {top: number, left: number, bottom: number, right: number} 

**scrollsToTop** bool 

When true, the scroll view scrolls to top when the status bar is tapped. The default value is true.

**showsHorizontalScrollIndicator** bool 

**showsVerticalScrollIndicator** bool 

**stickyHeaderIndices** [number] 

An array of child indices determining which children get docked to the top of the screen when scrolling. For example, passing `stickyHeaderIndices={[0]}` will cause the first child to be fixed to the top of the scroll view. This property is not supported in conjunction with `horizontal={true}`.

**style** style 

├─[**Flexbox...**](http://facebook.github.io/react-native/docs/flexbox.html#proptypes)
├─**backgroundColor** string
├─**borderBottomColor** string
├─**borderColor** string
├─**borderLeftColor** string
├─**borderRadius** number
├─**borderRightColor** string
├─**borderTopColor** string
├─**opacity** number
├─**overflow** enum('visible', 'hidden')
├─**rotation** number
├─**scaleX** number
├─**scaleY** number
├─**shadowColor** string
├─**shadowOffset** {h: number, w: number}
├─**shadowOpacity** number
├─**shadowRadius** number
├─**transformMatrix** [number]
├─**translateX** number
└─**translateY** number

**throttleScrollCallbackMS** number 

**zoomScale** number 

The current scale of the scroll view content. The default value is 1.0.