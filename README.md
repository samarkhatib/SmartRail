# SmartRail

Seat-occupancy sensing for trains, and two passenger interfaces built on it: a
platform screen that shows where seats are free along the train, and a phone app
that sends each passenger to one carriage so the crowd spreads over all doors.

**Team:** Yazan Rabea, Abed Azem, Samar Khatib. **Mentor:** Itai Dabran.
Project in Internet of Things, Technion ICST.

The full write-up is [Documentation/SmartRail_Report.docx](Documentation/SmartRail_Report.docx).
Its appendices are the guides for running and changing the code:

| Appendix | Covers |
|---|---|
| A - User guide for the simulation | Running `simulation/boarding_sim.py` and reading its output |
| B - Programmer guide for the simulation | Files, model structure, key functions |
| D - Stage 2 programmer guide | Firmware, dashboard server, HTTP API, tests |
| E - Stage 3 user guide | Platform screen, phone app, controls, sensors, phone access |

## Repository layout

    ESP32/             firmware (ESP32.ino, parameters.h) for the two IR seat sensors
    PC_software/       dashboard server, passenger interfaces, console tools, tests
    simulation/        boarding model, Israel Railways calibration, report figures
    Documentation/     the report and the technical documents it refers to
    Presentation/      stage 2 manuals, demo script, checklists, test template
    Poster/            final poster (PDF, PowerPoint) and its build script
    Unit Tests/        stand-alone sensor check sketch for the ESP32
    start_dashboard.bat / start_dashboard_network.bat   Windows launchers

## Quick start (Windows, Python 3.10 or newer)

From the repository root, in PowerShell:

    py -3 -m venv .venv
    .\.venv\Scripts\python.exe -m pip install -r .\PC_software\requirements.txt

Passenger interfaces (no hardware needed):

    .\.venv\Scripts\python.exe .\PC_software\passenger_routes.py

    Platform screen: http://127.0.0.1:8766/platform
    Phone app:       http://127.0.0.1:8766/app

Operator dashboard and controls, in a second console (or double-click
`start_dashboard.bat`):

    .\.venv\Scripts\python.exe .\PC_software\dashboard.py

    Passenger display: http://127.0.0.1:8765/
    Controls:          http://127.0.0.1:8765/controls

To open the passenger interfaces on a phone, add `--host 0.0.0.0` to the
`passenger_routes.py` command and browse to `http://<laptop IPv4 address>:8766/app`
from a phone on the same private network. `ipconfig` shows the address. The
controls page only opens on the laptop itself.

Tests (126 should pass):

    Push-Location .\PC_software
    ..\.venv\Scripts\python.exe -m unittest discover -s tests -p "test_*.py"
    Pop-Location

Simulation:

    .\.venv\Scripts\python.exe -m pip install -r .\simulation\requirements.txt
    Push-Location .\simulation
    ..\.venv\Scripts\python.exe boarding_sim.py
    Pop-Location

Results and charts are written to `simulation/results/`.

Firmware: open `ESP32/ESP32.ino` in Arduino IDE 2.x with the ESP32 core and
upload it to an ESP32 DevKit V1. Wiring is in
[Documentation/05_WIRING_AND_PINOUT.md](Documentation/05_WIRING_AND_PINOUT.md).
