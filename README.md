# Rohit-kumar_Front-End
# Q.1. Explain what the simple List component does?
The functional component in React called "List" is responsible for displaying a list of items, and it expects an array of objects that have a "text" property as a prop. Each object in the array represents an item in the list, and its "text" property contains the text to be displayed for that item.

To properly display each item, the List component includes a child component called SimpleListItem. SimpleListItem receives various props such as "index", "isSelected", "onClickHandler", and "text" to ensure that the item element is displayed correctly. SimpleListItem is responsible for rendering an item element with certain properties, including changing the background color to green if the item is selected and red otherwise. When an item is clicked, the onClickHandler function is called with the index prop to update the selected item.

WrappedListComponent is another child component defined in the List component. This component is responsible for rendering the entire list of items by mapping over the array of objects and rendering a SimpleListItem component for each item. WrappedListComponent expects the "items" prop, which is an array of objects with a "text" property, similar to the List component.

WrappedListComponent uses the useState hook to keep track of the currently selected item. Whenever an item is clicked, the selected index is updated using a handleClick function. Additionally, the useEffect hook is used to reset the selected index to null when the items prop changes.

Lastly, the List component renders an element with the list of SimpleListItem components that are rendered by WrappedListComponent. In conclusion, the List component and its child components provide a flexible and customizable way to render a list of items in a React application.

# Q.2. What problems / warnings are there with code?

1. In the SingleListItem component, the onClickHandler prop should be passed as a function reference, rather than invoking it immediately. The line should be changed from:
```
onClick={onClickHandler(index)} to
onClick={() => onClickHandler(index)}
```
2. In the WrappedListComponent component, the selectedIndex state hook is not initialized with a value, which can lead to unexpected behavior. The line should be changed from:
```
const [setSelectedIndex, selectedIndex] = useState(); to
const [selectedIndex, setSelectedIndex] = useState(null);
```

3. In the WrappedListComponent component, the items prop should be defined as an array of objects with a text property, rather than an array with a shape of an object. The line should be changed from:
```
items: PropTypes.array(PropTypes.shapeOf({
text: PropTypes.string.isRequired,
})), to
items: PropTypes.arrayOf(PropTypes.shape({
text: PropTypes.string.isRequired,
})),
```

4. In the SingleListItem component, the isSelected prop is being passed as the value of selectedIndex, which is a boolean. It should be passed as a comparison between index and selectedIndex, like this:
```
isSelected={index === selectedIndex}  
```
# Q.3. Please fix, optimize, and/or modify the component as much as you think is necessary.

import React, { useState, useEffect, memo } from "react";
import PropTypes from "prop-types";

```
// Single List Item
const WrappedSingleListItem = ({ index, isSelected, onClickHandler, text }) => {
  return (
    <li
      style={{
        backgroundColor: isSelected ? "green" : "red",
      }}
      onClick={() => onClickHandler(index)} // modified
    >
      {text}
    </li>
  );
};

WrappedSingleListItem.propTypes = {
  index: PropTypes.number,
  isSelected: PropTypes.bool,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};

const SingleListItem = memo(WrappedSingleListItem);

// List Component
const WrappedListComponent = ({ items }) => {
  const [selectedIndex, setSelectedIndex] = useState(null); // modified

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);

  const handleClick = (index) => {
    setSelectedIndex(index);
  };

  return (
    <ul style={{ textAlign: "left" }}>
      {items.map((item, index) => (
        <SingleListItem
          key={index}
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex === index} // modified
        />
      ))}
    </ul>
  );
};

WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(
    // modified
    PropTypes.shape({
      text: PropTypes.string.isRequired,
    })
  ),
};

WrappedListComponent.defaultProps = {
  items: [
    { text: "FrontEnd | Steeleye" },
    { text: "BackEnd | Steeleye" },
    { text: "Data Engineer | Steeleye" },
  ],
};

const List = memo(WrappedListComponent);

export default List;
```
