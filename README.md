Q.1. Explain what the simple List component does?

The list react component renders a list of items and take array as a props and the each item is text of type String. This renders a list of items  items as <li> and each using SingleListItem which is a memorized version of WrappedSinglelistItem, which takes different props like index, isSelected, onClickhandler, text. Upon click an item it get selected and changes color to green and changes to red if not. This functionality is being checked by a Boolean value isSelected and each item has a different index and a required text.

Q.2. What problems / warnings are there with code?

1. The onClickHandler is called directly and will be invoked immediately and on each render. It should be called as a callback function.

2. In the WrappedListComponent the useState is undefined initially and should have ‘null’  instead of empty value.

3. In the SingleListItem component, the isSelected prop is should be passed as a comparison between index and selectedIndex 

4. In the WrappedListComponent the declaration for the items must be "PropTypes.arrayOf" and "PropTypes.shape" instead of "PropTypes.array" and "PropTypes.shapeOf"

Q.3. Please fix, optimize, and/or modify the component as much as you think is necessary.

import React, { useState, useEffect, memo } from "react";
import PropTypes from "prop-types";

// Single List Item
const WrappedSingleListItem = ({ index, isSelected, onClickHandler, text }) => {
  return (
    <li
      style={{
        backgroundColor: isSelected ? "green" : "red",
      }}
      onClick={() => onClickHandler(index)} // code modified
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
  const [selectedIndex, setSelectedIndex] = useState(null); code // modified

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
          isSelected={selectedIndex === index} //code modified
        />
      ))}
    </ul>
  );
};

// code modifed
WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(
    PropTypes.shape({
      text: PropTypes.string.isRequired,
    })
  ),
};

WrappedListComponent.defaultProps = {
  items: [
    { text: "Abhishek" },
    { text: "Choudhary" },
  ],
};

const List = memo(WrappedListComponent);

export default List;
