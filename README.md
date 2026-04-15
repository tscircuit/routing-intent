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

### Overview

Routing intent is placed on objects as JSON properties. These properties can be integrated
into XML (for tscircuit), JSON (for Simple Route JSON or [Circuit JSON](https://github.com/tscircuit/circuit-json)) or custom DSLs and
libraries. All concepts are portable to different DSLs or libraries.

Netlist information is captured separately and is not relevant to routing intent.

A simple illustrative selector syntax can be used to refer to pins on a chip via a string e.g. `U1.pin3`. Aliases
are also acceptable e.g. `"U1.UART0_TX"`. This is recommended syntax, but IDs or other references can
be used. This syntax is recommended to make algorithm debugging simpler and provide hinting for LLMs.

### Bus Annotation

#### Basic Usage

```json
{
  "buses": [
    {
      "name": "U1_SPI",
      // Can be non-exhaustive, the pin implies the trace or net 
      "pins": ["U1.MOSI", "U1.MISO", "U1.GPIO1", "U1.GPIO2"]
    }
  ]
}
```

#### Differential Pairs

Differential pair pins are specified as pairs. Often a "bus" is just a single differential pair. Often buses are
composed a several sets of differential pairs. Again, we use a single pin reference rather than providing both pins.

```json
{
  "buses": [
    {
      "name": "HDMI",
      "differential_pair_pins": [
        ["HDMI.TMDS2_P", "HDMI.TMDS2_N"],
        ["HDMI.TMDS1_P", "HDMI.TMDS1_N"],
        ["HDMI.TMDS0_P", "HDMI.TMDS0_N"],
        ["HDMI.TMDS_CLK_P", "HDMI.TMDS_CLK_N"]
      ],
      "pins": [
        "HDMI.CEC",
        "HDMI.HEAC_P",
        "HDMI.DDC_SCL",
        "HDMI.DDC_SDA",
        "HDMI.HPD",
        "HDMI.PWR_5V",
        "HDMI.GND"
      ]
    }
  ]
}
```

### Geometric Trace Hints

The following table lists all geometric trace hints that are accepted:

| Trace Hint Type | Description |
| ------ | ---- |
| `trace_should_pass_near_point` | Trace should pass near a point |
| `trace_should_be_near_path` | Trace should be near a path |

- `point` uses `x`, `y` and layer. [example `layer` enum](https://github.com/tscircuit/circuit-json/blob/main/src/pcb/properties/layer_ref.ts)
- X/Y coordiates are from "board origin"
- Required means the autorouter must obey the constraint
- Weak means the autorouter should prefer to drop the constraint early, or still explore outside of it

```json
{
  "trace_hints": [
    {
      "hint_type": "trace_should_pass_near_point",
      "trace": "U1.pin1->U2.pin2",
      "point": { "x": "30mm", y: "20mm", layer: "top" },
      "close_enough_distance": "2mm",
      "too_far_distance": "10mm",
      "required": false,
      "weak": true
    }
  ]
}
```

```json
// TODO
```

### Geometric Bus Hints

```json

```






