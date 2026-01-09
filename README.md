# HarnessForge - Wire Harness Design Tool

Version: 3.1.x 

[harnessforge.app](https://harnessforge.app)

HarnessForge is a React JSX based Wire Harness Design Tool. It bridges the gap between logical electrical schematics and physical loom layouts. Designed for automotive, motorsport, and DIY electronics projects, it allows users to design wiring circuits, route physical bundles, and automatically calculate wire lengths.
It uses TailwindCSSv3 and the lucide-react icon set.

![HarnessForge Screenshot](images/screenshot.png)

# Example Project Files
* [Simple Injector Loom](ExampleFiles/Simple%20Injector%20Loom.json)
* [Demo Aux Loom](ExampleFiles/Demo%20Aux%20Loom.json)
# Example Connectors (with diagrams)
* [BMW 8HP Transmission](Connectors/BMW/8HP%20Trans%20Connector_Template.json)
* [BMW 8HP 10Pin Shifter](Connectors/BMW/A90%208HP%20Shifter%2010Pin_Template.json)
* [Cantcu](Connectors/Cantcu/CANTCU%20Connector_Template.json)
* [Deutsch](Connectors/Deutsch/DT4.json)
* [Link G4X XtremeX A](Connectors/Link/Link%20G4X%20XtremeX%20A%20Connector-img_Template.json)
* [Link G4X XtremeX B](Connectors/Link/Link%20G4X%20XtremeX%20A%20Connector-img_Template.json)

# Key Features

* Dual-View Design: seamless toggling between Schematic (logical) and Loom (physical) views and the option to split view (both).

* Schematic:
    * Logical connections in the harness (pin to pin).
    * Auto wire routing for schematic diagrams.
    * Wires calculate the shortest path with auto collision avoidance. Wires to/from the same pin will stack.
    * Wire lengths are calculated based on the physical loom layout and bundle lengths.
    * Wire gauge and colour can be set (AWG).
    * Connectors can have notes and diagrams added.
* Loom:
    * Physical representation of the loom (connector to connector)
    * Define bundle lengths and physical routing.
    * Junctions: Place junctions to split your loom into multiple paths.
    * Splices: Splices defined in schematic view can be snapped to bundles and auto-calculate wire lengths before and after the splice.
    * Cut List Calculation: Automatically calculates wire lengths including a configurable service loop.

* Component Management: Support for Connectors (DT/Custom), Splices, Diodes, Resistors, Fuses, and Relays.
* Wire Circuit Highlighting: Show all connected pins on the circuit.
* Data Export/Import:
    * JSON Project saving/loading.
    * CSV Export for Bill of Materials (BOM) and Parts/Wire Cut Lists.
    * Connector Import/Export
    * Connector Diagram Import (1MB Max Size) for visual representation of connectors.
    * Print-ready PDF generation.


# Installation & Setup

For Local installations Electron release builds are provided for Linux (Deb/Appimage/tar.gz) and Windows (exe) it is also published online via Vercel at [harnessforge.app](https://harnessforge.app).

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
* Wire Highlighting: Click a wire and it will highlight showing its path through the schematic. If you use Split view it will also show its path through the harness.
* Wire Lengths: Requires placing connectors in loom view and placing any splices (orange diamonds) in their physical location on the loom.

## Working in Loom View
This is where you define how the wires are physically routed.
* Positioning: Components added in Schematic view appear here. Drag them to represent their physical location.
* Junctions: Click "Create Junction" to add anchor points (grey dots) for creating junction points in the loom.
* Bundling (Creating the Harness):
    * Click "Create New Bundle" (Link icon).
    * Click a source component or junction (Start point).
    * Click a destination component or junction (End point).
    * A "bundle" path is created.
**Note: Bundles have a source and a destination, when you change the length of a bundle it will push/pull the destination side away from the source side.**
* Auto-Routing: Wires created in the Schematic view automatically flow through these bundles using the shortest path algorithm.
* Splices: Drag a Splice node onto a Bundle. It will snap to the bundle, the location of these is critical as it will calculate the wire length either side of the splice.
**Note: Try and keep Splices one grid point away from (Bézier) curved corners as it can bug the calculated lengths in certain cases.**
* Bundle Length: Click a bundle to set its length in the properties field then press enter. Or just enable the Show Length toggle and drag it to the desired length.

## Wire Lengths & Calculations
The tool automatically calculates the wire length required to build the harness.
* Calculation Logic: Sum of Bundle Lengths + Service Loop (eg: 50mm).
* Visualizing Lengths: Toggle the Ruler Icon in the top bar to see physical lengths of bundles and wires on the canvas.
* Manual Adjustments: Click a Bundle to manually type in its physical length (mm) in the properties panel. This allows you to match the design to real-world measurements.
* Scaling: Set the pixels per mm in the Settings menu (default: 1), this allows scaling for physically larger or smaller looms. You may want to increase this value when working on smaller harnesses or decrease to 0.5 for larger harnesses.

**Note: Changing the scaling does not adjust the physical location of parts so you will need to manually adjust these.**

## FAQ
* Q: I have finished my schematic view now there are a bunch of parts connected by dashed lines on my loom page, what do I do now?
    * A: The dashed wires on the loom page represent the logical connections created by the schematic, in order to physicalise these you need to connect them with bundles. Bundles can either connect two components together or a component to a junction point. Use junctions as an exit point for you to split things out of the loom on a different path. The example files included demonstrate this.
* Q: I have run a wire with a splice on the schematics page but I am seeing "Not routed in Loom"
    * A: The Splice (diamond) objects need to be snapped into a bundle on the loom before wire lengths can be calculated with splices.
    * A Cont: Resistors and diode's are intentionally not included on the loom page as such wires running to these will always show "Not routed in Loom" and no lengths will be calculated.
* Q: I want to represent both sides of a connector of an associated harness but now my wire lengths are wrong.
    * A: In the **Schematic View** first make sure there are wires mapped to the appropriate pins (eg Pin 1 Male to Pin 1 Female, Pin 2 Male to Pin 2 Female etc.), once connected drag the connectors until they are physically next to eachother, if connectors are butted up next to eachother this will hide the connections underneath if you prefer.
    * A Cont: In the **Loom View** do not create a bundle joining them as this will effect wire lengths, there will be a dashed line (Air Wire) connecting them logically, just drag them side by side to hide this underneath. Wire pathing will continue on the other side of the connector to it's destination and wire cut lengths will remain correct.

## Known Issues
* Coords column in Parts list serves no purpose - This was a development tool for troubleshooting, will be hidden in future build.
* Windows Build Specific: Printing Schematic or Loom to PDF occasionally shows some 1 pixel tearing. Seems to be something specific to the Windows Electron build only.

## Screenshots
Schematic View
![HarnessForge Schematic Screenshot](images/screenshot-schematic.png)

Connections View
![HarnessForge Connections Screenshot](images/screenshot-connections.png)
