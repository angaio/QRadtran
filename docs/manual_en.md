# QRadtran Installation and User Manual

**Version** 0.3.0　　**Date** 2026-09-19　　**Publisher** Wuhan WaveFront Co., Ltd.

This manual is also available in 简体中文, Русский, 日本語, 한국어, Français, Deutsch and Español; after installation all versions are in the `doc\` folder of the installation directory.

QRadtran is atmospheric transmittance and radiance software for infrared and electro-optical system engineers. On Windows it computes, through a graphical interface, spectral transmittance along slant and horizontal paths, path and sky radiance, and the apparent radiance of a target. It supports multi-curve comparison, parameter sweeps that produce look-up tables (CSV / HDF5), a command-line tool and a Python interface. The computation kernel is the open-source libRadtran, installed together with the software; no separate configuration is needed.

---

## Contents

1. [System requirements](#1-system-requirements)
2. [Installation](#2-installation)
3. [First run](#3-first-run)
4. [Interface overview](#4-interface-overview)
5. [Parameter panel in detail](#5-parameter-panel-in-detail)
6. [Typical workflows](#6-typical-workflows)
7. [Parameter sweeps and look-up tables](#7-parameter-sweeps-and-look-up-tables)
8. [Command-line tool](#8-command-line-tool)
9. [Python interface](#9-python-interface)
10. [Settings](#10-settings)
11. [Computation method and accuracy](#11-computation-method-and-accuracy)
12. [Frequently asked questions](#12-frequently-asked-questions)
13. [Uninstalling](#13-uninstalling)
14. [Files and folders](#14-files-and-folders)

---

## 1. System requirements

| Item | Requirement |
|---|---|
| Operating system | Windows 10 / 11, 64-bit |
| Processor | 4 cores or more. The kernel runs several processes in parallel; more cores make batch calculations faster |
| Memory | 8 GB or more |
| Disk | About 1.2 GB after installation (includes the spectral tables for all three resolutions). An SSD is recommended; fine-resolution runs read a lot of data |
| Display | 1600 × 900 or larger |
| Other | No Python, MSYS2 or WSL required; the VC++ runtime is included |

The Python interface is optional and requires a system Python 3.10 64-bit with numpy.

---

## 2. Installation

1. Double-click `QRadtran-0.3.0-Setup-x64.exe`.
2. Choose the installer language (简体中文, English, Русский, 日本語, 한국어, Français, Deutsch, Español). The installer records the chosen language as the interface language of the software; it can be changed later under Tools → Settings.
3. Read the third-party licence notes and click Next.
4. Choose the installation folder, by default `C:\Program Files\QRadtran`. The path may contain non-ASCII characters, but a network drive is not recommended.
5. Additional tasks:
   - **Create a desktop shortcut**: checked by default.
   - **Add the install folder to PATH**: unchecked by default. When checked, `qradtran-cli` can be typed in any command prompt.
6. Click Install and wait for the files to be copied (about 1 to 3 minutes).
7. The final page can launch the software or open this manual.

The installer requires administrator rights. To install for the current user only, choose "Install for me only" in the privilege prompt; the default folder then becomes `%LOCALAPPDATA%\Programs\QRadtran`.

### Portable use

The installer is optional: copy the whole release folder `QRadtran\` anywhere and run `QRadtran.exe` from it. All dependencies are inside the folder; deleting the folder uninstalls the software.

---

## 3. First run

After start-up look at the status bar in the lower right corner of the window:

```
Kernel libradtran 2.0.6 | 7 processes
```

This line means the computation kernel is ready; "processes" is the number of kernel processes that run at the same time. If it says "Kernel unavailable", open Tools → Settings and check the kernel path, see section 10.

**Interface language**: by default the software follows the Windows display language and supports Simplified Chinese, English, Russian, Japanese, Korean, French, German and Spanish. Choose a language under Tools → Settings → Interface language and restart; the language can also be forced on the command line, for example `QRadtran.exe --lang en`.

For the first run start from a preset scenario:

1. Menu File → Load preset scenario…, choose `mls_ground_to_air_3_5um.json` (mid-latitude summer atmosphere, ground looking at a target at 10 km altitude, 3–5 µm).
2. Click Run on the toolbar or press F5.
3. A job appears in the Jobs and log panel at the bottom; when the progress bar completes, the curves appear in the centre: blue is transmittance (left axis), warm colours are radiometric quantities (right axis). The whole run takes about 7 seconds.

---

## 4. Interface overview

```
┌─────────────────────────────────────────────────────────────────────┐
│ File  Calculate  View  Tools  Help                                  │
│ [New][Open][Save] │ [▶Run][■Stop][Sweep] │ [Export image][Reset zoom] │ X axis  Density │
├──────────────┬───────────────────────────────────────┬──────────────┤
│  Parameters  │  Spectra / Results table              │  Curves      │
│  ▸ Atmosphere│                                       │  ☑ Transmitt.│
│  ▸ Geometry  │        (dual-axis spectral plot)      │  ☑ Path rad. │
│  ▸ Aerosol   │                                       │  colour/show │
│  ▸ Surface   │                                       │              │
│  ▸ Spectral  │                                       │              │
│  ▸ Outputs   │                                       │              │
│  [Run]       │  x = 4.35 µm  Transmittance 0.812 …   │              │
├──────────────┴───────────────────────────────────────┴──────────────┤
│  Jobs and log: job list (status / progress / time) │ log (kernel calls, warnings) │
├─────────────────────────────────────────────────────────────────────┤
│ Status bar: cursor read-out                     Kernel libradtran 2.0.6 │
└─────────────────────────────────────────────────────────────────────┘
```

The three dock panels can be dragged, closed and rearranged; a closed panel is restored from the View menu or by dragging the edge.

### Toolbar

| Button | Function |
|---|---|
| New / Open / Save project | A project is one JSON file holding all parameters and the computed results |
| Load preset scenario | Three example scenarios shipped with the software; their names follow the interface language |
| Run (F5) | Computes with the current parameters; the result is added to the plot without replacing earlier results |
| Stop all | Cancels every running job |
| Parameter sweep / LUT | Opens the batch dialog, see section 7 |
| Export image | Saves the current plot as PNG / PDF / JPEG |
| Reset zoom (Home) | Restores the full range of the plot |
| X axis | Unit of the horizontal axis: µm / nm / cm⁻¹; takes effect immediately |
| Spectral density | Spectral density unit of radiometric quantities: per µm / per nm / per cm⁻¹ |

### Plot interaction

| Action | Effect |
|---|---|
| Mouse wheel | Zoom around the cursor |
| Drag | Pan |
| Double-click | Reset |
| Move the mouse | The bottom line and the status bar show the value of every curve at the cursor |
| Check box in the Curves panel | Show / hide a curve |
| Double-click the colour swatch | Change the curve colour |
| Right-click a result name | Export that result as CSV, or remove it |

Transmittance is drawn on the left axis (0 to 1); radiance and irradiance are drawn on the right axis, so both can be overlaid.

---

## 5. Parameter panel in detail

### 5.1 Atmosphere

| Item | Description |
|---|---|
| Standard atmosphere | Tropical, mid-latitude summer, mid-latitude winter, sub-arctic summer, sub-arctic winter, US Standard 1976 (the six AFGL atmospheres), or Custom profile |
| Import profile (CSV) | Imports your own pressure/temperature/humidity sounding, see below |
| Water vapour column | When checked, scales the total precipitable water to the given millimetres; otherwise the value of the standard atmosphere is used |
| Ozone column | As above, in DU |
| CO₂ mixing ratio | ppm, default 420 |

**Custom profile file format**: CSV or whitespace-separated text with a header line. Required columns: altitude, pressure, temperature and one humidity column. Recognised column names (case-insensitive):

| Quantity | Column names | Unit |
|---|---|---|
| Altitude | z, alt, altitude, height | km (`z_m` for metres) |
| Pressure | p, pres, pressure | hPa (`p_pa` for Pa) |
| Temperature | t, temp, temperature | K (`t_c` for °C) |
| Humidity (one of) | rh; q; h2o / h2o_vmr | relative humidity %; specific humidity g/kg; volume mixing ratio ppmv |
| Ozone (optional) | o3, o3_vmr | ppmv |

Example:

```
z_km,p_hpa,t_k,rh
0.0,1013.0,295.0,70
1.0,900.0,289.0,65
2.0,795.0,283.0,55
5.0,540.0,262.0,40
10.0,265.0,223.0,20
```

Above the imported profile the levels are completed with the selected standard atmosphere.

### 5.2 Geometry

| Path type | Meaning | Parameters |
|---|---|---|
| Slant path (observer → target) | Observer and target at different altitudes | Observer altitude, target altitude, view zenith angle |
| Horizontal path | Observer and target at the same altitude | Observer altitude, horizontal path length |
| Whole column, vertical | Surface to top of atmosphere | None |
| Sky radiance only | No target; the radiance of the sky in a given direction | Observer altitude, view zenith angle, azimuth |

Angle conventions:

- **View zenith angle**: 0° straight up, 90° horizontal, 180° straight down. It must be below 90° when the target is above the observer and above 90° otherwise; the panel flags an error if not.
- **View azimuth**, **solar azimuth**: 0° north, clockwise.
- **Solar zenith angle**: affects only the solar-band radiometric quantities, not the transmittance.
- **Surface elevation**: the altitude of the bottom of the atmospheric profile; observer and target altitudes are above sea level.

For zenith angles between 85° and 95° the software switches to a layered extinction integration for near-horizontal paths and issues an accuracy warning.

### 5.3 Aerosol and cloud

| Item | Description |
|---|---|
| Aerosol type | None, rural, urban, maritime, tropospheric (Shettle models), or an OPAC mixture |
| Visibility | Horizontal surface visibility in km; sets the boundary-layer aerosol loading |
| Season | Spring/summer or autumn/winter profile |
| Stratospheric aerosol | Background / moderate volcanic / high volcanic / extreme volcanic |
| OPAC mixture name | Used with OPAC, e.g. continental_average, maritime_clean, urban, desert |
| Single cloud layer | Water or ice cloud with base, top, water content and effective radius |

### 5.4 Surface

| Item | Description |
|---|---|
| Surface albedo | Solar band, 0 to 1 |
| Surface temperature | Emission temperature of the surface in the thermal infrared |
| Surface emissivity | Thermal infrared, 0 to 1 |

### 5.5 Spectral

| Item | Description |
|---|---|
| Spectral unit | µm / nm / cm⁻¹. The limits are converted when the unit changes |
| From / to | Spectral range. The software sends the solar band (≤ 5 µm) and the thermal band (≥ 2.5 µm) to the kernel separately and joins the results |
| Output step | Output grid spacing; 0 uses the kernel's representative-wavelength grid directly |
| Band model resolution | Coarse 15 cm⁻¹ / medium 5 cm⁻¹ / fine 1 cm⁻¹ / LOWTRAN 20 cm⁻¹. Finer is more accurate and slower |
| Solver | DISORT (multiple scattering, default) or two-stream. When radiance is needed the software switches to DISORT automatically |
| DISORT streams | Default 16; increase to 32 for strongly scattering cases (thick aerosol, cloud) |

Run-time reference (4-core office PC): 3–5 µm ground-to-air, coarse about 7 s, medium about 30 s; 8–12 µm horizontal 1 km about 2 s.

### 5.6 Outputs

| Item | Meaning | Unit |
|---|---|---|
| Path transmittance | Spectral transmittance from observer to target | dimensionless |
| Path radiance | Radiance emitted and scattered into the line of sight by the atmosphere along the path | W/(m²·sr·cm⁻¹), switchable |
| Sky radiance | Sky radiance seen from the observer along the line of sight | as above |
| Direct / diffuse downward / upward irradiance | Irradiances at the observer altitude | W/(m²·cm⁻¹) |
| Target apparent radiance | For the given target temperature and emissivity, the radiance seen by the observer after atmospheric attenuation plus path radiance; the incident radiance at the target is output as well | W/(m²·sr·cm⁻¹) |

---

## 6. Typical workflows

### 6.1 Single run and comparison

1. Set the parameters and click Run.
2. Change one or two parameters (for example visibility from 23 km to 5 km) and run again. The new result is added to the same plot in a different colour, labelled with the scenario name.
3. Check or uncheck curves in the Curves panel on the right to compare them.
4. Switch to the Results table tab to read the numbers; the axis and spectral density units follow the toolbar.

### 6.2 Saving and restoring

File → Save project stores the current parameters and all results in one JSON file. Opening that file later restores the curves without recomputing.

### 6.3 Exporting

- Plot: File → Export image or the toolbar button, PNG / PDF / JPEG.
- Data: right-click a result in the Curves panel → Export this result as CSV. The CSV uses the axis and spectral density units currently selected on the toolbar; the header states the units.

### 6.4 Target detection scenario

To obtain the apparent radiance of a target at the detector:

1. Choose Slant path in Geometry and enter observer and target altitude and the zenith angle.
2. Check Target apparent radiance under Outputs and enter the target temperature and emissivity.
3. Running yields four curves: transmittance, path radiance, target apparent radiance and incident radiance at the target.
4. Subtract the Sky radiance only result for the same geometry from the target apparent radiance to obtain the target-to-background radiance contrast.

---

## 7. Parameter sweeps and look-up tables

Calculate → Parameter sweep / LUT… starts from the current parameter panel, computes every combination (Cartesian product) of the chosen parameter values and writes a look-up table.

### Dialog items

| Item | Description |
|---|---|
| Sweep dimensions | One parameter per row. Parameters are written as JSON pointers; the drop-down lists the common ones. Values can be a list `1, 2, 5, 10` or a range `0:10:2` (start:stop:step); string parameters are written by name, e.g. `Rural, Urban` |
| Column | Coordinate name of that dimension in the output file, defaults to the parameter name |
| Outputs | If nothing is checked, every quantity of the scenario is written |
| Band averages | Optional; band average and band integral over µm bands, written alongside |
| Output file | The extension selects the format: `.h5` for HDF5, `.csv` for a long table |
| Resume | Skips combinations already completed when re-running after an interruption |

The dialog shows the total number of combinations and the number of kernel runs per combination. The run can be cancelled; completed parts are kept.

### HDF5 file layout

```
/<Quantity>                   [dim1, …, dimN, spectral]   data, unit in attribute "units"
/bands/<Quantity>_average     [dim1, …, dimN, band]       band average
/bands/<Quantity>_integral    [dim1, …, dimN, band]       band integral
/coords/wavenumber            [spectral]                  cm⁻¹
/coords/wavelength_um         [spectral]                  µm
/coords/<column>              [values]                    coordinate of each dimension (number or string)
/coords/band                  [band]                      band names
/filled                       [dim1, …, dimN]             1 = combination computed
Root attributes: kernel, kernel_version, created_utc, base_scenario (base scenario JSON), plan (sweep plan JSON)
```

Reading in Python (requires h5py):

```python
import h5py
with h5py.File("lut.h5") as f:
    T = f["Transmittance"][...]            # shape [dim1, …, spectral]
    nu = f["coords/wavenumber"][...]
    tgt = f["coords/target_km"][...]
```

`pyqradtran\read_lut_h5.py` in the installation folder is a ready-made viewer script.

### CSV long table

One row per (dimension values…, wavenumber, quantity, value); band averages go to `<file>.bands.csv`. Suitable for Excel pivot tables or pandas.

---

## 8. Command-line tool

`qradtran-cli.exe` is in the installation folder; with "Add to PATH" checked it works from any location.

```
qradtran-cli example --out case.json         write a template scenario file
qradtran-cli validate case.json              check the parameters
qradtran-cli plan case.json                  list the kernel runs that would be executed, no computation
qradtran-cli run case.json --out out.csv     compute and write CSV (--unit um|nm|cm-1  --density cm-1|nm|um)
qradtran-cli run case.json --out out.json    write JSON (with metadata)
qradtran-cli batch-example --out plan.json   write a sweep plan template
qradtran-cli batch plan.json --out lut.h5    batch computation (.h5 or .csv; --resume to continue)
qradtran-cli kernel-info                     show kernel status and available resolutions
```

Exit codes: 0 success, 1 argument error, 2 validation failed, 3 kernel failure. Scenario files and GUI project files are interchangeable.

---

## 9. Python interface

`pyqradtran\` in the installation folder is a Python package built for Python 3.10 64-bit. Add the installation folder to `PYTHONPATH` before use:

```bat
set PYTHONPATH=C:\Program Files\QRadtran
python
```

```python
import pyqradtran as qr

s = qr.Scenario.standard("MidlatitudeSummer")
s.geometry.mode = "SlantPath"
s.geometry.observer_alt_km = 0.0
s.geometry.target_alt_km = 10.0
s.geometry.view_zenith_deg = 30.0
s.aerosol.type = "Rural"
s.aerosol.visibility_km = 23.0
s.spectral.set_range(3, 5, unit="Micrometer", step=0.01, resolution="Coarse")
s.outputs.transmittance = True
s.outputs.path_radiance = True

r = qr.run(s)
nu = r.wavenumber                   # numpy array, cm⁻¹
T = r["Transmittance"]
L = r.series("PathRadiance", per="Micrometer")     # converted to per µm
print(r.band_average("Transmittance", 3.0, 5.0))  # {'average':…, 'integral':…}
r.to_csv("out.csv", unit="Micrometer")

# parameter sweep
plan = qr.BatchPlan()
plan.base = s
plan.add_dimension("/geometry/target_alt_km", [1, 2, 5, 10], "tgt_km")
plan.add_band("MWIR", 3.0, 5.0)
qr.batch(plan, "lut.h5")
```

The full example is `pyqradtran\python_quickstart.py`. The bindings share the core library and kernel with the GUI, so the results are identical.

---

## 10. Settings

Tools → Settings:

| Item | Description |
|---|---|
| uvspec.exe / libRadtran data directory | Kernel location, by default `kernels\libradtran` under the installation folder. Normally no change is needed |
| Detect kernel | Shows the kernel version and the installed resolutions |
| Max concurrent kernel processes | 0 = automatic (CPU cores minus 1). Reduce when memory is short |
| Kernel run timeout | Seconds. Fine resolution, wide bands or many streams may need a larger value |
| Keep work directory on failure | Keeps the kernel input and output files for diagnosis when a run fails; the location is written to the log |
| Also keep work directory on success | For diagnosis only, normally off |
| Interface language | Follow the system, or fix one of the eight languages; takes effect after a restart |

Work directories are created under `%LOCALAPPDATA%\WaveFront\QRadtran\runs\`.

---

## 11. Computation method and accuracy

- **Kernel**: uvspec from libRadtran 2.0.6, run as a separate process; molecular absorption with the REPTRAN representative-wavelength band model (based on HITRAN 2004 and later), multiple scattering with DISORT.
- **Slant-path transmittance**: in the solar band from the ratio of the direct irradiances at the two altitudes; in the thermal band from the ratio of the radiance differences of two runs with a perturbed surface temperature.
- **Horizontal and near-horizontal paths**: the extinction coefficient is derived from the vertical transmittance of the layer and integrated along the spherical path.
- **Path radiance**: radiance at the observer minus the radiance at the target multiplied by the transmittance.
- **Accuracy**: benchmarked against MODTRAN4 in the long-wave and mid-wave atmospheric windows; in 6 of 8 cases the band-averaged window transmittance differs by less than 0.04, the best case by 0.003. Inside strong absorption bands the two generations of spectroscopic data differ systematically. See the validation report shipped with the software.
- **REPTRAN resolutions**: fine 1 cm⁻¹ is of the same class as MODTRAN's 1 cm⁻¹ band model; coarse 15 cm⁻¹ is suited to quick estimates.

---

## 12. Frequently asked questions

**The status bar says "Kernel unavailable" after start-up.**
The installation folder was moved or `kernels\libradtran` was removed by antivirus software. Check the paths in Settings; reinstall if necessary.

**The Run button is greyed out.**
A red validation message is shown below the parameter panel; fix what it says. Common causes: slant path chosen with target altitude equal to the observer altitude; zenith angle contradicting whether the target is above or below; identical spectral limits.

**The run fails and the log says "Netcdf error … reptran_…_medium".**
The data tables for the selected resolution are missing. The full installer contains all three; with a reduced package use Coarse or add the data.

**Thermal-infrared transmittance is all zero.**
Happened in versions before 0.2.0 with the two-stream solver; fixed — DISORT is used automatically when radiance is required.

**The calculation is slow.**
Fine resolution with a wide band and 32 streams is the slowest. Tune parameters with coarse resolution and produce the final plot with fine resolution; for batches use all available processes.

**The right-axis values are hard to interpret.**
The right axis shows radiometric quantities; the unit follows the toolbar's Spectral density setting: per cm⁻¹, per nm or per µm. Tables and CSV use the same setting.

**The interface is in English.**
When the Windows display language is not among the supported ones the software falls back to English. Choose the language under Tools → Settings → Interface language and restart.

**Python reports "DLL load failed".**
`PYTHONPATH` must point to the installation folder itself (the level containing `qradtran_core.dll` and `Qt6Core.dll`), not to the `pyqradtran` subfolder; Python must be 3.10 64-bit.

**Can I use my own libRadtran?**
Yes. Point uvspec.exe and the data directory in Settings to your version; it must be 2.0.x.

---

## 13. Uninstalling

Uninstall QRadtran from Settings → Apps, or run Uninstall QRadtran from the Start menu. Uninstalling keeps the user's project files and the run records under `%LOCALAPPDATA%\WaveFront\QRadtran`; delete them manually if desired.

For the portable variant simply delete the folder.

---

## 14. Files and folders

```
QRadtran\
├── QRadtran.exe              graphical interface
├── qradtran-cli.exe          command line
├── qradtran_core.dll         core library
├── Qt6*.dll, platforms\, styles\, translations\ …   Qt runtime
├── hdf5.dll, msvcp140.dll, vcruntime140*.dll        HDF5 and VC++ runtime
├── kernels\libradtran\
│   ├── bin\uvspec.exe        computation kernel (MinGW build) and its runtime
│   ├── data\                 atmospheric profiles, aerosol, cloud, solar spectra and REPTRAN tables
│   ├── source\               libRadtran source archive, patches and build script (GPL requirement)
│   └── VERSION
├── data\presets\             example scenarios
├── pyqradtran\               Python bindings and examples
├── licenses\                 third-party licence texts
└── doc\                      this manual (md and html in eight languages)
```

Project files (`.json`) and exported CSV / HDF5 / images are stored where the user chooses.

---

## Support

Wuhan WaveFront Co., Ltd.　　WeChat: angaio
