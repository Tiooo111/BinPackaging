## Entity

### Box
- width: int
- height: int
- id: int
#### Functions:
1. get_rotations():
  - Return: all available angles of the box. 
  - Currently, only two types (w, h) and (h, w) are considered.

### State
- width: int --- width of container
- height: int --- height of container
- boxes_to_place: List[Box] --- List of small boxes ready to be put into the container
- action_history: List[action] ---                               ]
- available_spaces: Dict[layer:int, List[available horizontal space: Interval]] --- A dictionary that tracks the available positions in the container. It maps a vertical level (layer) to a list of Interval objects, where each Interval represents a free space along that horizontal layer.
#### Key Functions:
1. clone():
  - Performs a deep copy of the current state. This is used when simulating actions, so the original state can be preserved while creating new states from performed actions.
2. get_possible_actions():
  - Return all possible actions that can be performed in the current state.
  - It iterates over the list of boxes that need to be placed, calculates their possible rotations, and checks each available position in the container to see if the box fits.
3. can_place_item():
  - Checks whether a box can be placed in a specified layer and interval.
  - It checks if the box's width fits within the available length of the interval and if the box's height fits within the container's vertical space.
4. split():
  - This function updates the available positions in the container after a box has been placed. The available space is split to account for the box's dimensions. It updates both the horizontal (x-axis) and vertical (y-axis) available space.
5. perform_action():
  - Executes the given action and generates a new state.
  - Calls split() to update available positions, and then merges adjacent free spaces in merge().
6. merge():
  - Merges adjacent available intervals on the same layer. If two adjacent intervals are next to each other (i.e., their end and start points match), they are combined into a single larger interval to reduce fragmentation.
7. evaluation():
  - Computes an evaluation score for the current state, based on several metrics:
    - total_area: The total area of boxes that have been successfully placed.
    - nums_available: The number of available intervals in the container.
    - least_layer: The lowest vertical level where there is still available space.