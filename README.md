# Routing Intent

Goal: Convey user intent (including geometric intent) to an autorouting algorithm in a “readable enough” way for AI tool calls

This does not include any directives that are well established and have DRCs, such as keepouts, antenna regions etc.

## Main Annotations

- Bus Annotations
- - Traces should be routed together, with similar lengths and spacing
- - Differential pairs are a subset of a bus annotation
- - A bus can contain differential pairs
- Geometric Bus and Trace Hints
- - A bus should go near or avoid an area/layer
- - A trace must pass through a point/layer
- Phase Annotations
- - Route these sets of traces before that set of traces
- Power
- - This pin requires a certain amount of power
- - This trace nominally carries a certain amount of power
- - Pins connected to this trace nominally require a certain amount of power
- - Power should be able to be conveyed in trace width for convenience/explicitness
- - Convey via escape strategies
- Pours and Layer Purpose
- - Convey a layer is a pour for a certain net
- - Convey a section or polygon within a layer is a pour for a certain net
- - Convey that a pour can be "broken" by signal traces
- Fanout
- - Convey that a chip should be fanned out


## JSON Representation

TODO
