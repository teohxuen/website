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

<div align="center">

| **Acronym** |        **Definition**         |
| :---------: | :---------------------------: |
|   **ASV**   |   Autonomous Surface Vessel   |
|   **AUV**   | Autonomous Underwater Vehicle |
|  **BBAS**   | Bumblebee Autonomous Systems  |
|   **BCB**   |     Battery Charging Box      |
|   **CAN**   |    Controller Area Network    |
|   **OCS**   |   Operator Control Station    |
|   **PCB**   |     Printed Circuit Board     |
|   **PMB**   |    Power Monitoring Board     |

</div>


---

## 1. Summary

TODO:

---

## 2. Problem Background
### 2.1 Introduction
Bumblebee Autonomous Systems (BBAS) is a student-led project team that designs and builds Autonomous Underwater Vehicles (AUVs) for the annual RoboSub competition. Following lessons learned from AUV4.1’s participation in RoboSub 2023, the team is concurrently developing AUV4.5 for RoboSub 2025 and AUV5.0 for RoboSub 2026 (Table 1).

![AUV4.1 at RoboSub 2023](RoboSub2023.jpeg)
##### Figure 1: AUV4.1 at RoboSub 2023

| **Competition** | **Vehicles Deployed** |
| :-------------- | :-------------------- |
| RoboSub 2023    | AUV4.1                |
| RoboSub 2025    | AUV4.5                |
| RoboSub 2026    | AUV4.5 and AUV5.0     |
##### Table 1: AUV Deployments in RoboSub Competitions

AUV4.5 retains the same hull design as AUV4.1 but features a newly developed electrical and software subsystem. This new electrical subsystem is designed to be mechanically compatible with the existing AUV4.1 hull while also serving as the baseline for AUV5.0. 

### 2.2 Current System
The current power system for the AUVs comprises two main components, the Battery Hull and the Battery Charging Box (BCB). Each Battery Hull houses a Power Monitoring Board (PMB) and a LiPo battery.

The PMB monitors and reports key telemetry data, such as the battery's voltage, current, battery hull's temperature and pressure, both on the screen and via the CAN Bus. Two such hulls are connected to power to the AUV. The waterproof designs allows for quick battery swap during pool test by eliminating the need to unseal the main vehicle hull.

![Battery Hull](BatteryHull.png)
##### Figure 2: Metal 3D-Printed Battery Hull


![AUV4.1 PMB](AUV4.1PMB.png)
##### Figure 3: AUV4.1's Power Monitoring Board

![Assembled Battery Hull](assembled.png)
##### Figure 4: Assembled Battery Hull

The BCB comprises an AC-DC power supply and a LiPo charger. It includes connectors that interface directly with the battery hull, allowing the battery to be charged without removing it from the hull.

![Battery Charging Box](bcb.png)
##### Figure 5: Battery Charging Box

### 2.3 Limitations of Current System  
Deploying the current system at RoboSub 2023 identified several challenges.

#### 2.3.1 Challenges with Battery Hull Sealing
The limited space within the battery hull and the need for a watertight seal, makes sealing and unsealing the hull a challenging and time-consuming process. Without error, unsealing takes 2 members approximately 10 minutes, while resealing requires another 15 minutes. However, the confined space often results in disconnected cables during assembly, further complicating the process. This overhead increase downtime and the risk of assembly errors. 

![Sealing a Battery Hull at RoboSub 2023](sealing.png)
##### Figure 6: Two Man Team Attempting to Seal a Battery Hull at RoboSub 2023

#### 2.3.2 Limited Capabilities of Battery Fuel Gauge
The PMB currently uses BQ34110 chip as a gas gauge, which is designed for rarely discharged applications and lacks essential protection features [1] <Insert References>, reducing the AUV's reliability. Furthermore, the battery gauging capability was also not used, leading the team to rely on voltage readings to estimate the capacity of the battery. This method is inaccurate as the battery voltage remains relatively constant over a significant portion of the discharge cycle, while the actual capacity decreases rapidly (Figure 7). To err on the side of caution, the pool test is ended when the voltage drops to the nominal level, further limiting the AUV's runtime.

![Discharge Voltage Curve](voltagecapacity.webp)
##### Figure 7: Typical Li-Ion Discharge Voltage Curve [2]
<Insert Reference: https://www.ufinebattery.com/blog/useful-overview-of-lipo-battery-voltage/>

#### 2.3.3 Challenges with Tracking Battery Hull's Pressure and Temperature
During assembly, the battery hull is pressurised. Monitoring changes in pressure and temperature can alert the team to potential leaks. Currently, this tracking is done manually and only when a leak is
suspected. his reactive approach limits the availability of historical data. Manual tracking can lead to missing data or recording error, reducing the effectiveness of leak detection.

![Manually Logging Battery's Hull and Temperature on the Battery Hull's Lid](manualtrack.png)
##### Figure 8: Manually Logging Battery's Hull and Temperature on the Battery Hull's Lid

![Manually Logging Battery's Hull and Temperature on Excel](excel.png)
##### Figure 9: Manually Logging Battery's Hull and Temperature on Excel

### 2.4 Problem Analysis  
The limitations in the current system can be split into three overarching themes: poor user operability, limited runtime and reduced reliability. These themes and their associated limitations are summarised in Table 1.


<table>
  <thead>
    <tr>
      <th><b>Category</b></th>
      <th><b>Limitations</b></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="2"><strong>Poor User Operability</strong></td>
      <td>Challenges with Battery Hull Sealing</td>
    </tr>
    <tr>
      <td>Challenges with Tracking Battery Hull’s Pressure and Temperature</td>
    </tr>
    <tr>
      <td rowspan="2"><strong>Limited Run Time</strong></td>
      <td>Limitation of Battery Fuel Gauge</td>
    </tr>
    <tr>
      <td>Challenges with Battery Hull Sealing</td>
    </tr>
    <tr>
      <td rowspan="2"><strong>Poor Reliability</strong></td>
      <td>Limitation of Battery Fuel Gauge</td>
    </tr>
    <tr>
      <td>Challenges with Tracking Battery Hull’s Pressure and Temperature</td>
    </tr>
  </tbody>
</table>

##### Table 2: Categorisation of System Limitations

Poor user operability increases the likelihood of mistakes at competition, especailly under time constraints or operator fatigure. Furthermore, limited run time can lead to less testing opportunities and ultimately impacting competition performance (Table 2, Table 3). Finally, poor reliability increases the risk of mid-run failures or unplanned maintenace, leading to increased downtime.

|                           | **RobotX 2022** | **RobotX 2024** |
| :-----------------------: | :-------------: | :-------------: |
| Testing Time in Singapore |    200 Hours    |    330 Hours    |
|  Final Score (adjusted)   |      1450       |      4450       |

##### Table 3: Table of Competition Results and Testing Time in Singapore


|    **Team**     | **Final Score** | **Testing Time** |
| :-------------: | :-------------: | :--------------: |
|     **NUS**     |      4450       |     49 Hours     |
|    **KMOU**     |      2900       |     28 Hours     |
|    **ERAU**     |      2250       |     33 Hours     |
| **Inspiration** |      1600       |     25 Hours     |
|     **NTU**     |      1050       |     17 Hours     |

##### Table 4: Comparison of Final Score and Testing Time at RobotX 2024


---

## 3. Project Goal

As a competition team, the primary objective of our vehicle is to perform well at its competition (RoboSub). This can be attained by increasing the vehicle run time, improving user operability while ensuring a safe and reliable operations.

The project goal can be summarised as:

<div align="center">
    <b>
        To develop a battery management system that improves runtime, user operability and safety to enhance the AUV's reliability and performance.
    </b>
</div>

---

## 4. Design Considerations
  
### 4.1 Backward Compatibility

As outlined in Section 2.1, AUV4.5 has the same mechanical structure as AUV4.1. As such, the new PMB must remain mechanically and electrically compatible with the AUV4.1 battery hull. This backward compatibility ensures minimal modficiation to the existing support infrastructure when using the new PMB. Futhermore, designing to meet AUV4.1's specifications would also ensures forward compatibility with AUV5.0. This approach enables cross-platform use of the same PMB, reducing the need for custom variants and lowering the manufacturing cost by leveraging economies of scale.

To maintain backwards compatibility, the following constraints were identified.

| **Characteristics**  | **Constraints**                                                                               |
| :------------------- | :-------------------------------------------------------------------------------------------- |
| Dimensions           | The PMB and battery must fit within the internal volume of the AUV4.1 battery hull enclosure. |
| Electrical Interface | Existing connectors for power delivery and charging must be retained.                         |

##### Table 5: Design Constraints for Backward Compatibility


### 4.2 Functional Requirements  
In addition to physical compatibility, the PMB must meet or exceed the technical performance of its predecessor. The following functional requirements were defined to ensure the board can support AUV4.1, AUV4.5 and AUV5.0.

| **Technical Capabilities** | **Specifications**                                                                |
| :------------------------- | :-------------------------------------------------------------------------------- |
| Voltage Output             | 14.8V to 16.8V (4S)                                                               |
| Continuous Current         | 40A                                                                               |
| Communication Protocol     | CAN2.0                                                                            |
| Telemetry                  | Voltage, Current, Internal Pressure, Internal Temperature displayed on the screen |

The 40A current specification is based on the rated continuous current limit of the SubConn Low Profile Connector on the battery hull <Insert Reference https://www.macartney.com/connectivity/subconn/subconn-low-profile-series/subconn-low-profile-9-contacts/>. This provides sufficient headroom, as testing indicates a typical current draw of 11A per battery when moving at full-speed. CAN Bus is used for inter-board communication within the AUV, hence CAN bus integration is crtical.

##### Table 6: Core Functional Requirements for PMB

### 4.3 Component Standardisation

To reduce cost and simplify procurement, certain components of the PMB are shared with the other PCBs onboard the AUV. Specifically, the STM32F103C8T6 and the ISOW1044B isolated CAN Transciever (Table 7). This standardisation reduces design effort and chance of error as the same schematic capture and layout can be reused across the PCBs. This also allows for easier spare prepartion as the same component can act as spares for the different PCBs.

<img src="mcuschematic.png" alt="Common Schematic for STM32F103C8T6" style="max-width: 100%;">

##### Figure 10: Common Schematic for STM32F103C8T6

| **Component** | **Reason for Selection**                                                                                                          |
| :------------ | :-------------------------------------------------------------------------------------------------------------------------------- |
| STM32F103C8T6 | A widely used microcontroller that is onboard the "Blue-Pill" development board, ensuring high availability and supply stability. |
| ISOW1044B     | Combines the isolated regulator and CAN transceiver into one single component, saving space onboard the PCB.                      |

##### Table 7: Reasons for the Components Selected for Standardisation 
  

---  

## 5. System Design

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


---

## 6. Improve Runtime
- Insert description on why we want to increase run time and how to do it

### 6.1 Battery Upgrade
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
  
### 6.2 Accurate SoC Estimation
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

## 7. Enhance User Operability and Workflow
- Aim to make it easier for memerbs to oeprate and maintain vehicle to keep vehcile reliable despite long operation hours. Main goal is to reduce down time but icnreasing reliability. Help to prevent damage to battery hulls or vehicle. Prevent taking people away from development by fixing damges.

### 7.1 In-Hull Firmware Flashing
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
  
### 7.2 Remote Status Monitoring
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

## 8. Improving Reliability and Safety

### 8.1 Automatic Fault Detection Alert
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

### 8.2 Protection Features
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


### 8.3 PMB PCB Design
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

## 9. System Evaluation
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
  
  - Give example of current situation
      - only 1 set of functional battery -> means test for 1h45 mins
      - 1255 to 1415 Charging -> 1h20mins of charging
      - Increase run time huge priority
    - Table of Testing Time -> compared to RobotX Result (get from interim presentation)

---

## 10. Impact
  - To be deployed at RoboSub 2025
  - PCB Designed will be released to help other teams design their own PMB
  - Internet connected BCB was not seen at RoboSub, with teams moving towards battery pods, this is something that can be useful for them
  

---

## 11. Future Work

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

## 12. Conclusion

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

