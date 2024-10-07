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