---
title: "Development of A Battery Management System for AUV4.5" 
date: 2025-04-03
lastmod: 2025-04-04
author: ["Teoh Xu En"]
description: "This report documents the work done on developing the Battery Management System for an Autonomous Underwater Vehicle (AUV4.5) in preparation for RoboSub 2025."
summary: "This report documents the work done on developing the Battery Management System for an Autonomous Underwater Vehicle (AUV4.5) in preparation for RoboSub 2025."
editPost:
    URL: "https://bumblebee.sg/"
    Text: "Team Website"
showToc: true
disableAnchoredHeadings: false
---

## Acknowledgement

- Bumblebee Team, especially Elec Team to bounce idea with
- Zi Yang and Tan Chern Lin for providing feedback, Chen Jia Wei for troubleshooting. Marvin for giving the idea.
- Guk Yi Siong for helping in obtaining power consumption data
- Ananyan for  validating my mechanical ideas
- Ms Annie, Patrick, KK, Graham
- Parker Schless from Cornell AUV for his advice when I faced problems
- Eugene for taking time to review the project and providing advice
- School for providing the lab, lab equipment and resources
- sponsors for helping the team also
----
## List of Common Acronyms

TODO:

---

## 1. Summary

TODO:

---

## 2. Problem Background

- Introduction
  - Student Project TEam that develops AUV for RoboSub.
  - Previously sent AUV4.1 for RoboSub 2024 and is developing AUV4.5 for RoboSub 2025 and AUV 5.0 for RoboSub 2025
    - Insert Table and timeline of the different AUV

- Problem Context
  - Background
    - Battery Management System comprises of
      - Battery Hull -> PMB, Battery and Battery Hull
      - Battery Charging Box -> Charger
    - Designed such that the batteries are in its own waterproof hull so that it is easy to change battery at pool test
    - BCB is designed to support the current system by adapting the connectors to the charger
    - PMB is used to turn on and off the batter and report its voltage and current on the CAN Bus. 
  
  - Current Challenges
    - Difficult to seal
      - Increase down time
      - Bad user operation
    - Limited Capabilities of Battery Fuel Gauge
      - Poor Fuel Guaging (lack there off)
        - Reduce Run time
      - No Protection (It just monitors the voltage and current)
        - Reduce Reliability
    - No systematic monitoring of leak
      - Poor User Operation
      - Unreliable
  
  - Analysis of Problems
    - Bad user operation -> prone to mistakes at competition can cause reliability issue at competition
    - Increase down time and Reduce run time -> less testing can be done
      - Give example of current situation
      - only 1 set of functional battery -> means test for 1h45 mins
      - 1255 to 1415 Charging -> 1h20mins of charging
      - Increase run time huge priority
    - Table of Testing Time -> compared to RobotX Result (get from interim presentation)


---

## Project Goal

As a competition team, the primary objective of our vehicle is to perform well at its competition (RoboSub). This can be attained by reducing the vehicle’s down time and ensuring a safe, systematic operations

Refer to the Analysis of Problem

Project Goal:
To develop a battery management systems that improves runtime, user operability and safety to enhance the AUV's reliability and performance.

---

## Design Considerations

- Design Specification
  - Need to fit in the same hull (Backward Compatibility)
    - PMB size constraints
    - Have to use the same connector and pins
    - Becasue we deploying AUV4.5
    - Minimal modification allow for easy swap (get to work with new and old PMB)
    - Streamline spare prep and lower cost by leveraging Economies of Scale
  - Basic Criteria (can be presented in a table)
    - Able to power 40A continuous
    - Can spike X A
    - Able to communicate with rest of AUV
    - Same Interface
      - Reed Switch to turn on and off
      - Able to display key stats on screen
  - Ease of Production and Spares (competition experience)
    - Less stuff to pack
    - MCU, CAN Chip etc are standardised.
  

---  

## System Design

Provide a bigger picture first.
Summary of the system design, will go into details below how everything work etc. (how the design of the PMB and BCB PCB can help to fulfil the project goal while meeting design considerations)

- 2x PMB Architecture -> surrounded by dotted lines
  - Architecture should reference the PMB along with the different subsystem and external components
  - Talk about the differnt Power and Grounds
  - One is when connected in BCB
    - Show how the power goes to charger
    - CAN goes to PMB board
    - PMB board powerd by USB-C
    - Using Balance connector for programming pin
    - Data Flow
        - PMB -> CAN Bus -> ESP32 -> Telegram Bot -> Google App Script
  - One is when connected to AUV
    - Show that it communicate with CAN to AUV
    - Power with connector
  - Data Flow
    - PMB -> CAN Bus -> SBC-CAN -> Serial -> SBC -> Ethernet to Router -> Data Uploaded Online

- Using SystemPres, Relay and Reed Switch to turn on the vehicle
  - Turn on is via reed switch activiating a relay
  - Turn off is MCU detect reed switch and activate a signal
    - Allows MCU to do whatever it needds to do before pwoering off.


<!-- - Overall System Architecture
  - How is everythinkg linked together
    - Architecture Diagram
  - Say which thing can solve what
- Mention that alternatives were discussed below
- Create table of need to be done,must have etc -->

---

### Increase Run Time
- Insert description on why we want to increase run time and how to do it
- Battery Upgrade
  - Current Battery is 2 years old where with regular discharge
  - Insert data on battery degradation over time
  - We need 6 batteries for each AUV especially with the 2x AUV
  - Need procure new cost effective batteries
  - Shortlisted 3x Batteries
    - Same Voltage and Dimensions to fit in the hull and to be backward compatible
    - Power per dollar comparison
  - Li-Ion chosen for the higher capacity ina smaller form factor
  - Purchase 2x Raitan Battery for test
    - Insert table to compare the 2x battery runtime as well as dimensions comparison with weight
      - 2.4kg for 2 LiIon Battery, 2.5kg for 2 LiPo battery
      - Old: 188 x 75 x 42, New: 171 x 78 x 44
    - Insert photo of the battery in the hull
    - Compare Power Drain between old and new battery
  - Shown that the Raitan battery is a worthy replacement
  - The Molicell Chemistry is available by TI -> help make the learning process easier (check if the other chemistry have or not)
  
- Accurate SoC Estimation
  - Previously was using the gauging chip without using the learning algorithm
  - Means rely on voltage only
    - Inaccurate -> insert voltage to capacity graph to explain why is it difficult to stop based on voltage
  - By changing the chip we can implement RSOC tracking
    - Allow us to stop at a more accurate time
      - More time to draw
  - Say we do research found 2 other teams with battery gauging
    - OSU and Cornell -> one is using BQ40Z50 the other is using BQ40Z80 (double check the name)
    - Showed that it is utilised by teams with good results
      - Insert replies by Cornell on good review
    - Standalone with protection, gauging feaure
    - Software to allow for easy configuration of data flash and registers
      - Insert Photo of BQ Studio Interface
    - Lots of support from TI (forum)
    - Maybe can put a literature review portion to explain how the Impedance Tracking work and how the learning process works
  - Chose BQ40Z50 as it has a more accurate tracking
    - Insert comparison table with older chip
      - Able to support 4S
      - Lots of support online
    - Based on battery chemistry + learning to study
    - Insert power draw time difference
      - If using RSOC give us extra 20mins as compared to if we relied on voltage only
    - Look at their predicted charging and draw time, then compare how accurate it is
      - Charging (Case 1)
        - 8:20pm, predcited 70 minutes. Battery: 16.42V RSOC 84%, charging at 4A
        - completed at 9.10pm. Battery: 16.84V, RSOC 100%. Finished 20mins faster than expected
      - Charging (Case 2)
        - 1.07 AM, 128 Minutes left, battery at 65% and 15.76 V
        - 2.41 AM, charging completed. Finished 30mins faster than expected


  
---

## Improve User Operability and Workflow
- Aim to make it easier for memerbs to oeprate and maintain vehicle to keep vehcile reliable despite long operation hours. Main goal is to reduce down time but icnreasing reliability. Help to prevent damage to battery hulls or vehicle. Prevent taking people away from development by fixing damges.
- In-Hull Firmware Flashing
  - Allows for flashing and update without opening hull
    - Reduces error during reseasling, reduces downtime
    - Tested during RobotX 2024 where a USB port was exposed on the ASV to allow for firmware flashing
  - Few Option
    - CAN Bus Flashing to allow for flashing via CAN
      - Require higher overhead as you need a more expensive microcontroller
      - Need build own bootloader
      - Show old connector where CAN is alreaday exposed
    - Use the original Balance Connector
      - Explain that with the BQ40Z50 chip, balancing can be done with the chip, so balancing dont have to use charger to balance.
        - Frees up the connector
      - Reduce changes needed to be done
      - Allows to expose the BQ chip comms also
  - Insert image of old balance connector vs new balance connector
  
- Remote Status Monitoring
  - AS mentioned in xxx manual logging was used to track if battery hull is sealed
  - Difficult to determine slow leak
  - Upload relevant data online
  - This provide historical data for operator to reference easily without unsealing the hull
    - Can be useful for leak
    - Or to track degredation of battery
    - Or to track if cells are becoming unbalanced
      - All of which should be rectified to ensure that AUV battery can function properly
  - Few Option
    - MCU with Wireless Capability
      - Not suitable for Metal hull
    - Accompanying PCB for BCB
      - Same connector used to charge also hold the CAN Lines
      - Able to connect CAN Line to the PCB to retrieve data from the BCB
      - BCB would then be able to upload the data online
    - Chose the BCB option as we dont need the PMB to have access to the internet all the time. This is as when it is turned on, it is sually connected to the AUV or the BCB. When it is connected to AUV it would have access to the Internet
      - Insert diagram from interim report
  - While BCB means we need extra component, it is worth while as we can implement additional features (mentioned in section xxx) we can use it
  - Especially since the PMB will be on only when conencted to AUV or BCB
  - Go through BCB PCB design
    - Powered with USB-C allow to power with the Charger itself (no need extra plug)
    - Have ESP32 to get the CAN stuff and publish online with ESP capability
      - Insert CAN Table 
      - ![CAN Table](image.png)
    - Talk about how it receive the CAN messages and use the Google App Script to Publish into its relevant scripts
    - Test
      - Simulated with Serial Communication with the computer
    - Chose google sheet instead of database as it is easy for all members to use and track. Can even use on phone. 
  - current time obtained from NTP
  - Can say that previously there was plan for local logging -> included in te design of the PMB
      - As the data would be uploaded online by either the AUV or the BCB, there isnt really a need
      - Accessing the local data is more troublesome as you have to unseal the hull.
  - To ensure data accuracy there is a timeut for the stats
    - If any stats dont come within 2x its reporting frequency the data is cleared and not reported to prevent posting outdated data
  - Use a table to compare what is shown on the screen, what is reported to telegram and what is sent to google sheet
  - Explain the differences (difference in purpose)

---

## Improving Reliability and Safety

- Automatic Fault Detection Alert
  - Build on what was mentioned in the pervious section
  - Based on experience with ASV Tele Channel
    - Know that it is good
    - Telegram channel is commonly used by members so low barrier of entry
    - Allow to alert multiple people higher chance of someone noticing
  - The Google App Script when it uploads also check
    - If the Pressure below 107 report immediately on Telegram
    - Check with previous P/T to see if there is a significant drop -> likely leak -> will also alert member
    - Insert scatter plot of Pressure and Temperature measurement
  - Show telegram screenshot when leak might be detected
  - Also able to alert user that the battery is charged
  - Also good reminder to members that the battery is being charged since they would be receving messages (will remind them to turn off charger)

- Battery Protection
  - Used BQ40Z50 protection
  - Protects against possible electrical fault -> increase system robustness
  - Include literature review of hardware vs software safety
  - Tested with COV, OC
    - Tested both hardware and software limit
      - Insert data and photo and excel
    - also tested CUVC such that when the battery voltage is low and there is high current draw it will cut to protect the battery (to decide if I want to include this)
    - explain that connector limit is 40A (insert reference to SubConn)
    - While we have fuse, it is also good to have additional layer of protection
    - also has a short circuit protection
  - protect battery and auv from further harm


- Go through PMB Design (good practses to increase reliability)
  - Look at OneNote on PCB explanation
  - Power Rating
    - Show the load test
    - show temperature picture
    - Pool test show that it is around 11A per battery when moving at full spped
    - To be safe, the test was done at 40A because it was the connector rating (include time spent running)
    - Insert calculation for designs in appendix (refer to OneNote)
      - Talk about the Current Draw and Oz of outer layer as well as trace width
      - Show that it is 4 layer with split plane for the different Power
    - Refer the SLUA on how to route
      - Talk about the difference and similarity to SLUAA reccomendation
  - Isolation
    - Protection and noise especially for battery
  - Look at Altium to see what each section does and how it ties to the design
  - Go through mistakes that was in the PCB
    - and list the possible improvement also
  - Design for easy debugging
    - Test Points
    - Extra conector to test CAN Transmissions
    - Switches to switch communication line
  - Take a photo of PMB and highlight all the connections to show all the output and what it is connected to
  - Power Consumption chart to spec the different voltage regulator
  - Current draw of MCU and screen is 40mA

  
---

## System Evaluation and Impact

- Evaluation
  - Basic Feature
    - Refer to earlier table to see if able to meet all the requirements
    - Able to be backward compatible -> can fit into old battery hull, can be turned on with reed switch, can charge, can discharge
    - Able to report telemetry, able to draw, able to charge
      - Verified reporting is accurate -> Voltage measured with multimeter
      - Current is calibrated with a load tester drawing a constant load
      - Discharging and Charging State of Battery Verified
  - Improvements (use a before: after table)
    - Run Time: Increased
    - User Operability and Maintenace: 25 mins to 5mins to upload, constant logging
    - Reliability -> able to detect leak immediately
    - Have another table that compares old PMB and New PMB
      - Interfaces
      - Data Reporting
      - Ease of use etc.
  - Mistakes
    - No Pull Down for Isolator Signal
    - No pull up for I2C

- Impact
  - To be deployed at RoboSub 2025
  - PCB Designed will be released to help other teams design their own PMB
  - Internet connected BCB was not seen at RoboSub, with teams moving towards battery pods, this is something that can be useful for them
  

---

## Future Work

- Test with the AUV
  - Unable to do so due to limited hull and pool test
  - Did not want to open and swap out the PMB as the hulls are old and notoriously hard to seal
  - Was waiting for development of new hull to put the new pmb in
- Merge BCB into a Smart Charger
  - Communciate with PMB to negoitate desired current and voltage
  - Can get data directly from the charger also
- Better User Operability
  - Using same connector for Balance Pin and Flashing
      - works for previous PMB as it is connected to each other
      - Does not work as thsi fried everything
    - Future Work
      - Change connector to something different so you dont accidentally misconnect
      - More work and not interchangeable but its just crimp 1x so its not too bad
    - should ahve integrated protection into the line
      - clamping diode to prevent over 3v3
      - diode to prevent reverse polarity

---

## Conclusion

- Move from Power Monitoring to Power Management
- Well Integrated system to deliver xxxx AUV for RoboSub

---

## References

---

## Appendix

- PCB Schematic and Routing
- Calculations
  - Look at OneNote on PCB explanation
- BQStudio Configuration (Insert Link to the Page)

