# HarnessForge - Wire Harness Design Tool

Version: 3.1.x

HarnessForge is a React JSX based Wire Harness Design Tool. It bridges the gap between logical electrical schematics and physical loom layouts. Designed for automotive, motorsport, and DIY electronics projects, it allows users to design circuits, route physical bundles, and automatically calculate wire lengths.

![HarnessForge Screenshot](images/screenshot.png)

# Example Files
* [Simple Injector Loom](ExampleFiles/Simple%20Injector%20Loom.json)
* [Demo Aux Loom](ExampleFiles/Demo%20Aux%20Loom.json)
# Key Features

* Dual-View Design: seamless toggling between Schematic (logical) and Loom (physical) views.

* Auto-Routing:

* Schematic: Orthogonal auto-routing for wiring diagrams.
    * Loom: Uses Dijkstra’s algorithm to calculate the shortest path for wires through physical bundles with auto collision avoidance.
    * Physical Simulation:
        * Define bundle lengths and physical routing.
        * Floating Splices: Splices snap to bundles and auto-calculate their position ratio.
        * Cut List Calculation: Automatically calculates wire lengths including a configurable service loop.

*    Component Management: Support for Connectors (DT, Custom), Splices, Diodes, Resistors, Fuses, and Relays.

* Data Export/Import:
    * JSON Project saving/loading.
    * CSV Export for Bill of Materials (BOM) and Parts/Wire Cut Lists.
    * Connector Import/Export
    * Diagram Import (1MB Max Size) for visual representation of connectors.
    * Print-ready PDF generation.


# Installation & Setup

This application is a single-file React component.
Electron release builds are provided for Linux (Deb/Appimage/tar.gz) and Windows (exe) it is also published via Vercel.

# User Guide
## The Interface:
The application is divided into six main views, accessible via the left sidebar:

*    Schematic: The logical drawing board. Place components and draw wires pin-to-pin.
*    Loom: The physical assembly board. Define how wires are bundled together and routed through the vehicle/chassis.
*    Split: View Schematic and Loom side-by-side.
*    Parts: A tabular view of all components (Bill of Materials).
*    Connections: A tabular view of all wires (Cut List).
*    Settings: A List of settings for the project.

## Working in Schematic View
This is where you define what connects to what.

* Adding Components:
* In the "Add Part" menu (top-left).
* Select a component (Connector, Splice, Fuse, etc.).
* For Connectors, you can choose preset Deutsch (DT) common pin counts or define a custom pin count. If you have a connector saved as a template you can click Import Connector also.

* Wiring:
    * Click a component to see its pins.
    * Click the + (Plus) icon on a source pin.
    * Click in a blank area on the destination pin. (anywhere but it's plus icon)
    * A wire line will auto-route between them.

**Note: Wires have a source and a destination, keep this in mind for how you want the parts list to look.**

* Properties: Click any component or wire to open the Properties Panel on the right to change names, gauges, colors, add notes or import a diagram for the connector. This will also show you the calculated wire length.

## Working in Loom View
This is where you define how the wires are physically routed.
* Positioning: Components added in Schematic view appear here. Drag them to represent their physical location.
* Junctions: Click "Create Junction" to add anchor points (grey dots) for creating junction points in the loom.
* Bundling (Creating the Harness):
    * Click "Create New Bundle" (Link icon).
    * Click a source component or junction (Start point).
    * Click a destination component or junction (End point).
    * A "bundle" path is created.

**Note: Bundles have a source and a destination, when you change the length of a bundle it will push the destination side away from the source side.**

* Auto-Routing: Wires created in the Schematic view automatically flow through these bundles using the shortest path algorithm.
* Splices: Drag a Splice node onto a Bundle. It will snap to the bundle, the location of these is critical as it will calculate the wire length either side of the splice.

**Note: Try and keep Splices one grid point away from (Bézier) curved corners as it can bug the calculated lengths in certain cases.**

## Wire Lengths & Calculations
The tool automatically calculates the wire length required to build the harness.
* Calculation Logic: Sum of Bundle Lengths + Service Loop (eg: 50mm).
* Visualizing Lengths: Toggle the Ruler Icon in the top bar to see physical lengths of bundles and wires on the canvas.
* Manual Adjustments: Click a Bundle to manually type in its physical length (mm) in the properties panel. This allows you to match the design to real-world measurements.
* Scaling: Set the pixels per mm in the Settings menu (default: 1), this allows scaling for physically larger or smaller looms. You may want to increase this value when working on smaller harnesses.

**Note: Changing the scaling does not adjust the physical location of parts so you will need to manually adjust these.**

