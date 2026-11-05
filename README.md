# FSMTTM: Process Schematic Workbench

A single-file HTML workbench for mapping a team's process the way a circuit is designed, finding the bottleneck, and turning the model into an automation proposal.

Open `index.html` in any modern browser. Nothing to install and no server needed. Your work is saved in that browser's local storage. Use **Export / import** to keep versions as JSON or to move them to another machine.

## Sheets

| No. | Sheet | What it captures | Engineering idea |
|---|---|---|---|
| 00 | Overview | Status of every sheet, method map | |
| 01 | Charter | Trigger, pain, stakeholders, success metric | Problem definition |
| 02 | Context | Inputs, controls, outputs, mechanisms | IDEF0 block diagram |
| 03 | Flow | Steps, owners, times, waits, automation fit, plus a shadow timer | Netlist and signal flow |
| 04 | States | Lifecycle of one work item: states, events, guards | Finite state machine |
| 05 | Timing | Arrivals, work windows, cut-offs, slack | Timing diagram, setup margin |
| 06 | Loops | Variables and +/− causal links, with reinforcing and balancing loops detected | Feedback loops |
| 07 | Analysis | Pareto of effort, fit-vs-load matrix, critical path, design rule check | Bottleneck and critical path |
| 08 | Solution | Steps mapped to modules, build increments, PoC definition | Modular design, encapsulation |
| 09 | Meeting kit | Questions per meeting, including questions generated from gaps | Design review |
| 10 | Brief | One-page proposal and a 30-second pitch, copied as Markdown or Mermaid | Datasheet |

## Engagement sequence

1. **Kickoff** (30 min): agree the problem, the boundary and the success metric. Fills 01 and 02.
2. **Shadow** (1 to 2 h at the desk): time real items with the shadow timer. Fills 03.
3. **Deep dive** (45 min): rules, data, systems, statuses, cut-offs. Fills 04, 05 and 03.
4. **Playback** (30 min): confirm the map, the bottleneck and the loops. Fills 06 and 07.
5. **Sign-off** (30 min): agree the PoC scope, success criteria and asks. Fills 08 and 10.

Every gap the design rule check finds (an unknown format, a state with no exit, a window that runs past a cut-off) becomes a question in the meeting kit.

## Model assumptions

- Load per step = minutes per item × items per day × (1 + rework %).
- Automation fit (0 to 100) weights rules 35%, data format 25%, system access 20% and stability 20%.
- Estimated savings: automate 80%, assist 45%, templated handoff 50%, simplify first 15%. Replace them with PoC measurements.
- 1 FTE = 7.5 h per day; 21 business days per month.

## Data handling

Describe the process with field names and patterns. Keep client names, account numbers and trade identifiers out of the workbench. The bundled sample project (trade confirmation breaks) uses invented figures.
