hc6528 400_4 (ESRF ID22, 10-11 June 2026): the plot sets and publication sets of the project site
=====================================================================================================

These files are read by diff-ref-struct (the core `drs` and `drs-publish`): https://jmigartua.github.io/diff-ref-struct-site/

Layout expected (a project folder for the app):
  Data/        the 101 scans of hc6528_400_4_renamed.zip (or a link to the folder that holds them)
  Rebinned/    the scans rebinned by the core, for the maps (see below)
  Plots/       the plot sets (*.drv): the long scans against T, all scans and the 1270 C hold against time,
               and their map versions; each member's variable typed with its source
  Publish/     the publication sets (*.drp): esrf-series (projections), esrf-maps (maps), with the captions

Open a set:            drs plots open Plots/hold-1270.drv
Its projection:        drs plots project Plots/hold-1270.drv --step 25 --dx 0.15
The figures:           drs-publish build Publish/esrf-series.drp      (LuaLaTeX, Latin Modern)
The maps need the rebinned scans (write them as XYDATA, .xys; the core does not read back its own .xye):
  for f in Data/*.xye; do b=$(basename "$f" .xye)
    drs data op "$f" rebin step=0.01  mode=average --out "Rebinned/$b-rebin0p01.xys"
    drs data op "$f" rebin step=0.005 mode=average --out "Rebinned/$b-rebin0p005.xys"; done
In the app: open any .drv from the Plots module (Projection, Map, Surface views).
