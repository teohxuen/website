---
title: "Battery Learning Cycle for BQ40Z50" 
date: 2025-04-03
lastmod: 2025-04-04
author: ["Teoh Xu En"]
description: "This documents the steps required to successfully complete a learning cycle for new batteries attached to BQ40Z50."
summary: "This documents the steps required to successfully complete a learning cycle for new batteries attached to BQ40Z50."
editPost:
    URL: "https://www.youtube.com/watch?v=WAcxrGDb68g"
    Text: "Video Guide"
showToc: true
disableAnchoredHeadings: false
---
## Preface

Note that this is **not** part of CDE4301 Final Report but documentation for future team members working on the Power Management Board.


---

Insert Reference to the Training Document

Switch off SMBus Comms to MCU JIC!
0) check that batteries are fully charged
1) Calibrate Current
1b) See how much current when PMB is active vs inactive (MCU side ON vs MCU side OFF)
2) Check all settings mentioned in learning PDF (MAKE SURE ALL SETTING RELATING TO CURRENT AND CAPACITY IS HALF) (Check Parker Docs Again)
Bare Minimnum (Design Capacity, Design Voltage, Taper, DSG Current, CHG Current, Quit Current, Term Voltage)
2b) Make sure that the correct Battery Chemistry is Flashed.
3) Turn off Protection on setting page (make sure scan is turned on)
3b) Turn on autoexport .gg.csv for every 30mins
4) Click on GAUGE_EN and check that GAUGE_EN bit is set.
5) Then click on RESET (Turn on Log)
5b) Verify Update Status is 04
6) Discharge until empty (C/4?)
7) Wait until REST Bit is Set
8) Charge Cell to Full using nominal charge rate, charge termination is reached when VCT = 1
9) Let cell rest until REST bit is set
10) In Data Memory>Gas Gauging>Update Status Register should equal 05
11) Check RDIS = 0
12) Discharge cell at C/5 rate until terminate voltage is reached
13) Let cell rest until REST bit is set
14) In Data Memory>Gas Gauging>Update Status Register should equal 06
15) Resistance Table and QMax should be updated