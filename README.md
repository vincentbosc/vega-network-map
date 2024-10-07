# vega-network-map

## Overview

This project leverages Vega to build a dynamic network map visualization within Kibana. It provides an interactive way to visualize network devices, their connections, and the health status of each element. The visualization also automatically organizes devices into network zones if the corresponding zone parameter is filled in for each device.

![network map image example](<img/example.png>)

## JSON Breakdown

### Signals

Signals define dynamic variables that control the behavior of the visualization. These include parameters such as the size of images, padding, margins, and grid opacity.
- **grid_opacity**: Controls the opacity of the grid background (default value: 0.1). The grid helps placing the devices on the network map. Can be set higher during development, and lower (or even at 0) when the network map is finished.
- **device_image_size_L** and **device_image_size**: Define the size of the device icons (large and default).
- **device_text_margin_top**: The margin above the text labels for devices.
- **zone_padding_x** / **zone_padding_y**: Padding around zones in the X and Y directions.
- **health_rect_width** / **health_rect_height**: Dimensions for health status rectangles.
- **health_rect_padding**: Padding around the health status rectangles.
- **health_rect_left_margin** / **health_rect_top_margin**: Margins for positioning health rectangles.

### Data

#### gridline

Defines the grid lines.

#### image_type

Lists different device types (e.g., user, firewall, switch, server) and provides URLs for the corresponding icons used to represent each device type.

#### devices

Specifies the individual devices on the network map, including their positions (x, y coordinates), type (e.g., user, firewall, server), hostname, label, and related metadata. For example, devices like firewalls or switches are represented with their respective icons and associated with network zones.
- **x** / **y**: The positions of each device on the network grid.
- **type**: The type of device (e.g., user, firewall, server). Linked to **image_type**.
- **name**: Internal name of the device.
- **zone**: Defines which zone the device belongs to (optional).
- **labe**l: Displayed information for the device.
- **hostname**: Used to link to the health status.
- **url**: Optional URL to link to more information about the device (e.g. another dashboard).

The transform section in this block defines calculated fields like the label’s vertical position and image size based on device type.

#### links

Defines the connections between devices, showing how network devices are linked. Each link contains a start and end device.
- **start**: The name of the starting device for the connection. Uses the **name** field of devices above.
- **end**: The name of the device that the link connects to. Uses the **name** field of devices above.

The transform uses a lookup to match the device’s positions and ensure the links are correctly rendered between devices.

#### zones

Based on **devices** data.
Automatically groups devices into their respective zones based on the zone property in the devices dataset. It calculates the boundaries (min_x, max_x, min_y, max_y) for each zone and applies padding.

#### health

- Retrieves health status data from an Elasticsearch index. The health information (such as CPU, memory, and storage status) is linked to the devices, and this data is used to visualize the health of each device on the map.
- The health information is periodically fetched from Elasticsearch, and devices are updated with their current health metrics like CPU and memory usage. 

This is the dynamic part of the network map. We chose here to represent cpu and memory but it could be anything.

#### cpu, memory, storage

Based on **health** data
Defines specific health metrics for devices. Each of these datasets transforms the raw health data and prepares it for display, such as computing positions for health rectangles and generating tooltips that display the percentage of resource usage.

### Scales

- **xscale** and **yscale**: Linear scales that map the X and Y values to the respective width and height of the visualization. These scales ensure the correct positioning of devices, links, and zones on the grid.

### Marks

Defines the visual elements (or “marks”) that will be rendered on the network map. Each mark describes how a specific type of visual element, such as lines, rectangles, or text, should be drawn.

#### Gridlines

Renders the grid lines at regular intervals across the X and Y axes based on the **gridlines** dataset.

#### Zones

Draws background rectangles for each network zone based on the calculated boundaries for each zone in the **zones** dataset.

#### Links

Renders dashed lines between connected devices as defined in the **links** dataset. These lines represent the communication paths between devices.

#### Device icons

Displays the icons for each device (e.g., firewall, server, switch) at the positions specified in the **devices** dataset. The images are pulled from URLs defined in the image_type dataset.

#### Device label

Renders the labels for each device below its icon. The position is calculated based on the **device_text_margin_top** signal and the **devices** dataset.

#### Health Rectangles (CPU, Memory, Storage)

Rectangles are drawn beside each device to represent the CPU, memory, and storage usage as colored rectangles. The fill color reflects the status (e.g., green for healthy, red for critical), and tooltips provide more detailed information.

