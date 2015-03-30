ListView - A core component designed for efficient display of vertically scrolling lists of changing data. The minimal API is to create a `ListView.DataSource`, populate it with a simple array of data blobs, and instantiate a `ListView` component with that data source and a `renderRow` callback which takes a blob from the data array and returns a renderable component.

Minimal example:

```html
getInitialState: function() {
  var ds = new ListViewDataSource({rowHasChanged: (r1, r2) => r1 !== r2});
  return {
    dataSource: ds.cloneWithRows(['row 1', 'row 2']),
  };
},

render: function() {
  return (
    <ListView
      dataSource={this.state.dataSource}
      renderRow={(rowData) => <Text>{rowData}</Text>}
    />
  );
},
```

ListView also supports more advanced features, including sections with sticky section headers, header and footer support, callbacks on reaching the end of the available data (`onEndReached`) and on the set of rows that are visible in the device viewport change (`onChangeVisibleRows`), and several performance optimizations.

There are a few performance operations designed to make ListView scroll smoothly while dynamically loading potentially very large (or conceptually infinite) data sets:

* Only re-render changed rows - the hasRowChanged function provided to the data source tells the ListView if it needs to re-render a row because the source data has changed - see ListViewDataSource for more details.

* Rate-limited row rendering - By default, only one row is rendered per event-loop (customizable with the `pageSize` prop). This breaks up the work into smaller chunks to reduce the chance of dropping frames while rendering rows.

##Props 

[**ScrollView props...**](http://facebook.github.io/react-native/docs/scrollview.html#proptypes)

**dataSource** ListViewDataSource 

**initialListSize** number 

How many rows to render on initial component mount. Use this to make it so that the first screen worth of data apears at one time instead of over the course of multiple frames.

**onChangeVisibleRows** function 

(visibleRows, changedRows) => void

Called when the set of visible rows changes. `visibleRows` maps { sectionID: { rowID: true }} for all the visible rows, and `changedRows` maps { sectionID: { rowID: true | false }} for the rows that have changed their visibility, with true indicating visible, and false indicating the view has moved out of view.

**onEndReached** function 

Called when all rows have been rendered and the list has been scrolled to within `onEndReachedThreshold` of the bottom. The native scroll event is provided.

**onEndReachedThreshold** number 

Threshold in pixels for onEndReached.

**pageSize** number 

Number of rows to render per event loop.

**removeClippedSubviews** bool 

An experimental performance optimization for improving scroll perf of large lists, used in conjunction with `overflow: 'hidden'` on the row containers. Use at your own risk.

**renderFooter** function 

() => renderable

The header and footer are always rendered (if these props are provided) on every render pass. If they are expensive to re-render, wrap them in StaticContainer or other mechanism as appropriate. Footer is always at the bottom of the list, and header at the top, on every render pass.

**renderHeader** function 

**renderRow** function 

(rowData, sectionID, rowID) => renderable Takes a data entry from the data source and its ids and should return a renderable component to be rendered as the row. By default the data is exactly what was put into the data source, but it's also possible to provide custom extractors.

**renderSectionHeader** function 

(sectionData, sectionID) => renderable

If provided, a sticky header is rendered for this section. The sticky behavior means that it will scroll with the content at the top of the section until it reaches the top of the screen, at which point it will stick to the top until it is pushed off the screen by the next section header.

**scrollRenderAheadDistance** number 

How early to start rendering rows before they come on screen, in pixels.