# VDI 3682 XML Schema

XML Schema Definition (XSD) for the Formalized Process Description (FPD) according to VDI/VDE 3682.

## Overview

The VDI/VDE 3682 guideline defines a graphical notation for describing technical processes. However, it lacks a standardized serialization format for software-independent data exchange. This schema provides an XML representation of the FPD information model.

### Schema Structure

- `project` – Root element containing project metadata, processes, and optional cross-system flows
- `process` – Process views with states, process operators, technical resources, and flows
- `state` – Products, Energy, and Information with identification and characteristics
- `processOperator` – Transforms input states into output states
- `technicalResource` – Resources used by process operators
- `flow` – Connections between states and process operators (entry/exit)
- `crossSystemFlows` – Optional container for State-to-State flows across process boundaries

## Files

| File | Description |
|------|-------------|
| `FPD_Schema.xsd` | Core schema defining the FPD information model (includes cross-system flow definitions) |
| `FPD_Visual_Extension.xsd` | Extension for visual/graphical layout information |
| `FPD_Complete_Schema.xsd` | Combined schema importing core and visual extension |

## Cross-System Flows

In practice, technical systems often interact — the output of one system becomes the input of another. The VDI/VDE 3682 standard models each system as a self-contained process with its own system limit, but does not prescribe how to represent connections between systems.

The `crossSystemFlows` extension in `FPD_Schema.xsd` provides a minimal, backwards-compatible mechanism for this:

- Cross-system flows are defined at the **project level** (outside of any process)
- Only **State-to-State** connections are permitted (reflecting logical material/energy/information transfer)
- Each flow explicitly references both the **source and target process** for unambiguous ID resolution
- The extension is **optional** — existing documents without cross-system flows remain valid

### Example

```xml
<fpb:project xmlns:fpb="http://www.vdivde.de/3682">
  <fpb:projectInformation entryPoint="dosing_module"/>

  <fpb:process id="dosing_module">
    <fpb:systemLimit id="sl_1" name="Dosing Module v01"/>
    <fpb:states>
      <fpb:state stateType="product">
        <fpb:identification uniqueIdent="P5" longName="Exhaust Gas"/>
        <!-- ... -->
      </fpb:state>
      <!-- ... -->
    </fpb:states>
    <!-- processOperators, technicalResources, flowContainer -->
  </fpb:process>

  <fpb:process id="filtration">
    <fpb:systemLimit id="sl_2" name="Filtration"/>
    <fpb:states>
      <fpb:state stateType="product">
        <fpb:identification uniqueIdent="P201" longName="Input Gas"/>
        <!-- ... -->
      </fpb:state>
      <!-- ... -->
    </fpb:states>
    <!-- processOperators, technicalResources, flowContainer -->
  </fpb:process>

  <fpb:crossSystemFlows>
    <fpb:crossSystemFlow id="csf_1" flowType="flow"
        sourceProcess="dosing_module" sourceState="P5"
        targetProcess="filtration" targetState="P201"/>
  </fpb:crossSystemFlows>
</fpb:project>
```

## Implementation

This schema is implemented in [FPB.JS](https://github.com/HamiedNabizada/FPB.JS), a web-based modeling tool for VDI 3682.

**Try it online:** [dev.fpbjs.net](https://dev.fpbjs.net)

## Reference

For more details, see:

> H. Nabizada, T. Jeleniewski, A. Köcher, and A. Fay, "Vorschlag für eine XML-Repräsentation der Formalisierten Prozessbeschreibung nach VDI/VDE 3682," in *17. Fachtagung EKA - Entwurf komplexer Automatisierungssysteme*, 2022.
>
> [ResearchGate](https://www.researchgate.net/publication/361951781_Vorschlag_fur_eine_XML-Reprasentation_der_Formalisierten_Prozessbeschreibung_nach_VDIVDE_3682)

```bibtex
@inproceedings{nabizada2022fpd,
  author    = {Nabizada, Hamied and Jeleniewski, Tom and Köcher, Aljoscha and Fay, Alexander},
  title     = {Vorschlag für eine XML-Repräsentation der Formalisierten Prozessbeschreibung nach VDI/VDE 3682},
  booktitle = {17. Fachtagung EKA - Entwurf komplexer Automatisierungssysteme},
  year      = {2022}
}
```

## Contact

For questions or feedback, feel free to open an issue or contact the authors.
