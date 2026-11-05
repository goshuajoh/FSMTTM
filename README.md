# FSMTTM: Process Schematic Workbench

A single-file HTML workbench for an automation solutions developer. It maps a team's process the way a circuit is designed, finds the bottleneck, turns the model into a proposal, and then carries the design through to a robot the service team can run: data, platform, step-by-step design, controls, failure modes, tests, go-live checklist, runbook and the Solution Design Document (SDD).

Open `index.html` in any modern browser. Nothing to install and no server needed. Every ticket is a project; projects are saved in that browser's local storage. Use **Projects, export, import** to switch projects, keep versions as JSON, or move a project to another machine.

## Lifecycle

The bar under the project name tracks the phase of the ticket: Intake, Discover, Design, Build, Test, Deploy, Handover, Closed. Each phase has a gate on sheet 00 that lists what "done" means, computed from the sheets. "What next" shows the first open gate items and any errors.

## Sheets

| No. | Sheet | What it captures | Engineering idea |
|---|---|---|---|
| 00 | Overview | Phase gates, what next, status of every sheet, method map | |
| **1 · Discover** | | | |
| 01 | Charter | Trigger, pain, stakeholders, success metric | Problem definition |
| 02 | Context | Inputs, controls, outputs, mechanisms | IDEF0 block diagram |
| 03 | Flow | Steps, owners, times, waits, automation fit, plus a shadow timer | Netlist and signal flow |
| 04 | States | Lifecycle of one work item: states, events, guards | Finite state machine |
| 05 | Timing | Arrivals, work windows, cut-offs, slack | Timing diagram, setup margin |
| 06 | Loops | Variables and +/− causal links, with reinforcing and balancing loops detected | Feedback loops |
| 07 | Analysis | Pareto of effort, fit-vs-load matrix, critical path, design rule check | Bottleneck and critical path |
| **2 · Design** | | | |
| 08 | Solution | Steps mapped to modules, build increments, PoC definition | Modular design, encapsulation |
| 09 | Data | Datasets with grain and keys; lineage from sources through the base table to outputs | Data flow diagram, star schema |
| 10 | Platform | The stack (Python, Power Automate, Power Apps, Power BI, Fabric, Automation Anywhere, Alteryx, Excel/VBA, Copilot/AI, SharePoint/Teams) scored per module from the fit signals; your choice and reason | Technology selection |
| 11 | Design | One row per build step: input → function → output, platform, toolbank function, loop and lookup (complexity), error policy, check | Interface specification |
| 12 | Controls | Accuracy controls with tolerance and evidence; failure modes with likelihood, impact, detection, response, alert and prevention; risk matrix | FMEA, fail-safe design |
| **3 · Deliver** | | | |
| 13 | Plan | Iterations with playbacks, test cases, go-live checklist | Bring-up plan, acceptance test |
| 14 | Runbook | How to run, inputs, outputs, checks, logs, exit codes, what to do when it fails, re-run, contacts | Operating manual |
| **4 · Communicate** | | | |
| 15 | Meeting kit | Questions per meeting, including questions generated from gaps | Design review |
| 16 | Brief | One-page proposal and a 30-second pitch, copied as Markdown or Mermaid | Datasheet |
| 17 | SDD | The full Solution Design Document assembled from every sheet, as Markdown or print to PDF | Design documentation |

## Engagement sequence

1. **Kickoff** (30 min): agree the problem, the boundary and the success metric. Fills 01 and 02.
2. **Shadow** (1 to 2 h at the desk): time real items with the shadow timer. Fills 03.
3. **Deep dive** (45 min): rules, data, systems, statuses, cut-offs. Fills 04, 05 and 03.
4. **Playback** (30 min): confirm the map, the bottleneck and the loops. Fills 06 and 07.
5. **Sign-off** (30 min): agree the PoC scope, success criteria and asks. Fills 08 and 16.
6. **Design review** (45 min): walk through data, platform, steps, controls and failure handling. Fills 09 to 12.
7. **Handover** (30 min, then one supervised run): hand over the runbook. Fills 13 and 14.

Every gap the design rule check finds (an unknown format, a state with no exit, a dataset without a key, a design step without an error policy, an output without a control) becomes a question in the meeting kit for the right meeting.

## Drafts and generators

- **Draft from flow** (sheet 11) turns the automatable steps on sheet 03 into build steps with a trigger, module, platform and a default error policy.
- **Add standard controls** (sheet 12) adds schema and completeness controls per source, a duplicate control per base table, a control total per output and an idempotency control for action steps.
- **Add standard failure modes** (sheet 12) adds the usual ways a robot fails: input missing or late, layout changed, target system down, action repeated on re-run, second copy started, crash midway, config missing, business-rule exceptions, volume spike.
- **Add standard tests** (sheet 13) adds the happy path, the negative tests, one unit test per decision step, peak volume, UAT and the parallel run.
- The **runbook** and the **SDD** are generated from the sheets and copied or downloaded as Markdown (diagrams as Mermaid). Print or save as PDF uses the browser.

## Toolbox

The design sheet suggests functions from the [Toolbox](https://github.com/goshuajoh/Toolbox) `toolbank` package per module (read, validate, transform, decide, act, report, trigger), and the runbook uses the exit codes of its robot template: 0 ok, 2 business exceptions, 3 system failure, 4 config error, 5 robot error, 6 already running. Error policies follow the same split: a business error skips the item and records it; a system error retries and aborts if it persists.

## Model assumptions

- Load per step = minutes per item × items per day × (1 + rework %).
- Automation fit (0 to 100) weights rules 35%, data format 25%, system access 20% and stability 20%.
- Estimated savings: automate 80%, assist 45%, templated handoff 50%, simplify first 15%. Replace them with PoC measurements.
- 1 FTE = 7.5 h per day; 21 business days per month.
- Platform fit scores start from a base score per module and move with the signals: screen-only access, API or file access, unstructured data, judgment, fixed rules, volume above 5,000 rows a day, and the input formats (email, PDF, portal, feeds). They are a starting point, not a decision.
- Complexity: batch work is O(N); a keyed lookup per item is O(N); a scan per item is O(N×M).
- Risk = likelihood × impact (1 to 5 each); 10 or more needs prevention.

## Data handling

Describe the process with field names and patterns. Keep client names, account numbers and trade identifiers out of the workbench. The bundled sample project (trade confirmation breaks) uses invented figures.
