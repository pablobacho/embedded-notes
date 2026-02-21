# Obsolescence Risk Assessment Implementation Guide

By Pablo Bacho

Updated on Jun 16th, 2022.

This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International](http://creativecommons.org/licenses/by-sa/4.0/) License.

![CC-BY-SA 4.0 International](https://licensebuttons.net/l/by-sa/4.0/88x31.png)

This is a derivative work of "Obsolescence Risk Assessment Process Best Practice" by F.J. Romero Rojo: F J Romero Rojo et al 2012 J. Phys.: Conf. Ser. 364 012095. Published originally under a [Creative Commons Attribution 4.0 International](https://creativecommons.org/licenses/by/4.0/) license.

I found the work by F.J. Romero outstanding, very realistic and practical. I adapted it to my use-case, thus the process I describe herein is not exactly the same. I encourage you to read the original work as well and come up with your own conclusions.

**Table of contents**

- [Introduction](#introduction)
  - [Subject](#subject)
  - [Scope](#scope)
  - [Methodology](#methodology)
- [Obsolescence Risk Assessment Report](#obsolescence-risk-assessment-report)
  - [Introduction](#introduction-1)
    - [System under review](#system-under-review)
    - [Revision history](#revision-history)
    - [Methodology](#methodology-1)
  - [System Support Plan](#system-support-plan)
  - [Resource planning](#resource-planning)
  - [Bill of Materials Prioritization](#bill-of-materials-prioritization)
    - [Preliminary Filtering](#preliminary-filtering)
    - [Probability of Becoming Obsolete](#probability-of-becoming-obsolete)
    - [Impact Criticality](#impact-criticality)
    - [Obsolescence Risk Level](#obsolescence-risk-level)
  - [Mitigation Decisions](#mitigation-decisions)
    - [Obsolete Materials](#obsolete-materials)
    - [Very High Risk of Obsolescence](#very-high-risk-of-obsolescence)
    - [High Risk of Obsolescence](#high-risk-of-obsolescence)
    - [Moderate Risk of Obsolescence](#moderate-risk-of-obsolescence)
    - [Low Risk of Obsolescence](#low-risk-of-obsolescence)
  - [Risk Record Update](#risk-record-update)
  - [Review](#review)


## Introduction

### Subject

In long-lifecycle projects, obsolescence has become a major problem as it prevents the maintenance of the system. This is the reason why obsolescence management must become an essential part of the product
support activities. In order to manage obsolescence effectively, it is essential to perform an obsolescence risk assessment of the Bill of Materials.

### Scope

The key factors that have to be analysed in the obsolescence risk assessment process for each component in the Bill of Materials are: number of manufacturers, stock available, state of technology and operational impact criticality. Very high risk components require taking immediate action. On the contrary, components with a lower risk require other strategies to be taken, such as monitoring or even a completely reactive approach.

### Methodology

This document is an implementation guide to create an Obsolescence Risk Assessment Report. The report contains all the information gathered during the assessment, obsolescence risk levels as well as specific actions to be taken to mitigate potential problems. The report is a standardized method of collecting, assessing and recording obsolescence risk information, serving as a support document for the project and team members involved, and within the organization itself.

## Obsolescence Risk Assessment Report

### Introduction

#### System under review

Identify the system or electronic board under review: Board name, revision, part number, etc.

#### Revision history

A revision history of the report, including dates and a brief text explaining the reason and main changes.

#### Methodology

The report must link to this implementation guide, so future revisions of it follow the same process, criteria and good practices described herein.

When necessary, deviations from this implementation guide, additional criteria or considerations used for this particular system must be documented in this section.


### System Support Plan

The first step should be to consider the time for which the system has to be sustained. It is necessary to take into account the planning for mid-life upgrades and also what subsystems are likely to be modified or replaced at this stage. This will allow identifying the period for which each component in the bill of materials (BoM) is required.


### Resource planning

This step in the process is intended to identify the resources available to manage obsolescence that can be allocated to the project:

* People (e.g. obsolescence managers)
* Tools (e.g. obsolescence monitoring tools)
* Budget for obsolescence management

As these resources are limited, the obsolescence risk assessment should enable to decide the key components for which these resources should be used to minimize the impact of obsolescence in the system’s performance and support costs.

At this stage it is necessary to align the resources with the strategy selected to meet the contractual terms for obsolescence management.


### Bill of Materials Prioritization

The first task shall be breaking down the system or equipment into manageable portions. The level of detail to go down to should be the lowest practical level, which is the lowest procured material. This could be either an electronic component, a mechanical part, or a submodule.

The first step is extracting the bill of materials (BOM) of the system.

#### Preliminary Filtering

From the BoM, the Obsolescence Manager shall filter out the obvious low risk components. This will allow reducing efforts that can be misspent if trying to undertake an exhaustive risk assessment for
every component in the BoM. Any components that meet any of the following criteria should be filtered out:

* Standard design (open architecture, standard connectors, modular)
* Passive
* Mechanical

The list of removed items should be reviewed by the Design Authority, the Repairing Supervisor and/or Program Manager to check if any component is single-sourced or critical for operation. In that case, the component should be brought back to the filtered BoM for further analysis.

A complete bill of materials must be kept for the record, including the risk analysis information. The complete bill of materials shall be added to the assessment report as an appendix or an external reference.

The obsolescence risk for each component in the filtered BoM should be assessed according to the following parameters: **Probability of becoming obsolete** and **Impact Criticality**.

#### Probability of Becoming Obsolete

Probability of becoming obsolete and turning into an obsolescence issue. This can be assessed considering the number of manufacturing sources, current stage in its life-cycle, stock level available at suppliers and state of technology.

Note: This part differs significantly from F.J. Romero's original work. Specifically, his work considers the stock available *in hand*, while I look at the stock available from suppliers; he compares it to the consumption rate, which I ignore because "stock level available" is already a subjective measure that depends on consumption rate (thus it is implied); he also looks at the *Years to End of Life* metric, which I find almost impossible to get most of the time, hence I replaced it by "current stage in its life-cycle" (new, active, NRND, last-time-buy, obsolete) and "state of technology" (old, cutting edge... which gives you a hint of for how long it might stick around).

Materials manufactured by 2 or more sources can be classified as LOW probability of becoming obsolete.

2+ manufacturers --> LOW probability of becoming obsolete

They do not need to be exactly the same part. As long as they have a drop-in replacement from a different manufacturer, we can consider them the same for the purpose of this indicator. E.g., it is very common to find Opamps or LDOs using the same packaging and pinout across manufacturers and, even if they have different specs, as long as they meet the requirements of our project they can be considered drop-in replacements.

Materials from a single manufacturing source must be analysed further to assess their probability of becoming obsolete. This analysis shall look at the stage the material is in in its life-cycle (Active, Not Recommended for New Designs, Last-Time-Buy, Obsolete, etc.), the stock level at the suppliers (which gives us an idea of "popularity"), and the state of the technology in the manufacturer's offering (is it their latest addition or the oldest offered?).

To do this, first, we need to look for PCNs (Product Change Notices) and PDNs (Product Discontinuance Notices) from the manufacturers.

* If a PDN has been issued, the materials must be classified as OBSOLETE (regardless of whether we can still buy it or not).
* If material has been declared NRND (Not Recommended for New Designs), it shall be classified as HIGH probability of becoming obsolete.

For materials declared as ACTIVE by the manufacturer, the table below can be followed:

Indicator       | Old tech  | Current tech
-----------     | ----------| -----------
**Low stock**   | HIGH      | MODERATE
**High stock**  | MODERATE  | MODERATE


The *stock level* can be assessed comparing it to the stock level of similar materials (similar function, similar features). E.g., the way I normally do it is by going to Digikey's website, filtering all the components meeting my main requirements (excluding packaging), ordering them by stock level, and seeing if my component is within the top 25%.

The *state of technology* can be assessed by looking at the manufacturer's offering. If the manufacturer offers components that can be considered "a new version" of the one we use, it is old tech. E.g., at the time of writing, using a Xilinx Spartan-6 FPGA when the Spartan-7 has been available for years. The Spartan-6 is still active, but if Xilinx decides to clean up their catalog, which one will go first?


#### Impact Criticality

Operational Impact Criticality of the obsolescence issue on the system's functioning and performance. It represents the potential loss of system's availability or capability.

Type                | Criticality
------------------- | ------------
Operation critical  | HIGH
Local impact        | MODERATE
Low impact          | LOW

A material is "Operation critical" when its potential obsolescence would imply huge changes in the system. E.g., an MCU becomes obsolete and not just the PCB needs to be updated, but also its firmware, interfaces with other components, even impacting other related equipment, triggering new cumbersome verification and validation processes.

"Local impact" can be used when the potential obsolescence problem of a material means minor changes in the system. E.g., a voltage regulator becomes obsolete and replacing it means changing its footprint on the PCB and a few passive components around it.

Use "Low impact" when the material can be easily replaced without risking availability, project deadlines or budget.


#### Obsolescence Risk Level

The Probability can then be combined with the Operational Impact Criticality to determine the obsolescence risk for that component, using the risk matrix below:

.                     | LOW Criticality | MODERATE Criticality  | HIGH Criticality
--------------------- | --------------- | --------------------  | ----------------
Declared OBSOLETE     | OBSOLETE        | OBSOLETE              | OBSOLETE
HIGH probability      | MODERATE        | HIGH                  | VERY HIGH
MODERATE probability  | MODERATE        | HIGH                  | HIGH
LOW probability       | LOW             | MODERATE              | HIGH

After the assessment, the bill of materials shall be ordered by risk level from higher to lower.

### Mitigation Decisions

After assessing the bill of material risk levels, decisions must be taken to minimize the impact of an obsolescence problem.

#### Obsolete Materials

Obsolete materials demand immediate action. Some strategies could be:

* Immediate redesign
* Risk mitigation buy (if stock still available)

Since resources are limited (whether in time, money or people), its impact criticality shall be considered to avoid spending more resources than really necessary to fix the problem.

#### Very High Risk of Obsolescence

These components should receive paramount attention, and mitigation strategies should be deployed to reduce the probability and impact that obsolescence issues may have in the system. Examples of mitigation strategies are as follows:

* Redesign
* Plan system upgrades
* Risk mitigation buy

Mitigation strategies shall contain specific actions to be taken within a specified time frame.

#### High Risk of Obsolescence

The level of effort required by these components is lower than the category above, but the Obsolescence Manager may decide to apply the analysis indicated in the previous section if there are enough resources available to perform it. At least, potential actions need to be analysed and considered, even if a timeline is not specified. Think of it as a "plan B".


#### Moderate Risk of Obsolescence

The components contained in this category should be monitored, so that any obsolescence issue can be managed proactively. The level of proactivity shall be according to the resources available.


#### Low Risk of Obsolescence

For these components, a fully proactive approach is neither appropriate nor cost effective. Therefore, it is advised for these components that obsolescence issues are dealt with reactively.


### Risk Record Update

Once the decisions about the level of proactiveness have been made and mitigation strategies have been decided for all the components (where applicable), they need to be recorded and implemented. A risk register has to be kept up to date, providing for each component in the BoM the following information:

* Current status (obsolete, active, NRND, etc.).
* Risk level
* Details about all the parameters considered in Risk Assessment (number of sources, stock level, criticality, etc.).
* Mitigation strategies decided (e.g. risk mitigation buy, monitoring, partnering agreements with supplier)

When using an ERP (enterprise resource planning), consider adding the status information to the materials, so it gets updated "automatically" for other bill of materials within the organization.

### Review

Periodically, the assessment needs to be reviewed and updated if necessary. A review date must be set according to the state of the project and the assessment results. The goal is to detect potential obsolescence problems early enough to fix them without risking the project's commitments.

E.g., during the development phase a review could be set before starting the circuit's layout, and/or before ordering the first prototypes. During production phase, a review could be set using a fixed timeline, or being event-driven such as stock in hand going below a certain threshold.

The Obsolescence Manager can decided whether this review shall include the whole bill of materials, or just a subset, such as very high risk components, or with a high impact criticality. This decision must be made taking into consideration the current stage of the project and the resources available to the obsolescence risk management program.

Review's coverage, timelines or conditions, must be documented in the Obsolescence Risk Assessment Report.