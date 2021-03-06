# NormalizedHeightCollectionViewLayout

Smooth scrolling solution to UICollectionView cells with varying height. When cells in a row have different heights, the height of the shorter cells will be adjusted to the height of the tallest cell in the row.

This approach uses manual mathematical calculations (without any call to `systemLayoutSizeFittingSize:`) to maximize performance and allow calculations from the background thread if necessary.

The use of auto layout outlet collection in the Interface Builder makes it possible to achieve this system -- there is no need to declare separate constants (or magic numbers) for margins and paddings, which greatly reduces maintenance cost. Calculated heights are cached wherever appropriate to minimize calculations.

## Features

<img src="demo.gif" width="375">

* Layout switching
* Append (single or multiple) new data with animation
* Remove (single or multiple) existing data with animation
* Customizable column count for portrait and landscape mode
* Customizable separator size

## Requirement

Any Xcode that can compile Swift 4.1.

## Installation

Drag the 'NormalizedHeightCollectionViewLayout' folder into your project.

## Usage
(Please see demo project under 'Demo' folder for more details)

* Create an instance of `NormalizedHeightCollectionViewLayout` and specify the type of cell that you want to use for measurement
```
lazy var gridLayout: NormalizedHeightCollectionViewLayout = {
    let layout = NormalizedHeightCollectionViewLayout()
    layout.measurementCell = gridMeasurementCell // gridMeasurementCell is subclass of UICollectionViewCell initialized from nib
    ...
```

* (OPTIONAL) Customize the number of column and size of separator according to your requirement. Specifying separator size of 0 will hide them entirely)
```
    ...
    layout.portraitColumnCount = 2
    layout.landscapeColumnCount = 4
    layout.verticalSeparatorWidth = 1
    layout.horizontalSeparatorHeight = 0
    return layout
}()
```

* Assign a specific model to your `NormalizedHeightCollectionViewLayout` instance, which will become the data source of the height calculation in the next point
```
var models: TrackableArray<T> = TrackableArray() { // replace 'T' with your Type here
        didSet {
            gridLayout.models = models
        }
    }
...
```

* Conform your `UICollectionViewCell` subclass to `HeightCalculable`, and implement the corresponding method to manually calculate the height of the cell given a specific width

## Limitations

The following limitations may be addressed in the future if necessary:
* The layout class interface only accept one kind of cell
