# Steel Fabrication Plant Simulation - Arena advanced tools 
**Developed with Arena Simulation Software & SAP Crystal Reports**

![image](https://github.com/user-attachments/assets/dae9d9fe-58b6-45d0-bb2f-0c45306a792f)

## Project Overview
This project, carried out during my BSc in Industrial Engineering at SUPSI (Lugano, Switzerland), simulates a steel fabrication plant handling beam cutting and drilling for structural metal components. The model aims to replicate real-world logic, optimize workflows, and provide insights into productivity and bottlenecks without altering the physical plant.

We will analyze performance data using SAP Crystal Reports, balance the plant to optimize resource allocation, and study all machine downtimes to build accurate stochastic distributions of MTTF (Mean Time To Failure) and MTTR (Mean Time To Repair) for each resource.

## Objectives
- Simulate the full process flow of the steel fabrication line
- Measure productivity (tons/hour), buffer saturation, WIP, and component counts
- Analyze failures and resource usage (MTTF/MTTR)
- Implement and compare dispatching rules
- Identify and solve production bottlenecks

## Plant Structure
![image](https://github.com/user-attachments/assets/948b3ffc-3d73-4910-b8be-2a4a5274e4f9)


### Machines
- **Sawing Machine**: Cuts beams into components based on the production plan. Uses buffers and conveyors for material handling.
- **Drilling Machine**: Three-head drill used for components requiring holes.
- **Combined Machine**: Performs both sawing and drilling in a single operation.
- **Overhead Conveyor System (Dorsale)**: Segmented (R1 to R4), motorized sections to move components between machines.
- **Buffers**: Manage intermediate storage between conveyor and machine infeed/outfeed.

### Material Flow
- Beams are loaded from Buffer B0
- Components are cut and optionally drilled
- Output depends on component length and required operations:
  - Only cut → processed by Saw or Combined
  - Cut + Drill → must go through Combined if any piece is < 1200mm
- Components < 1200mm are ejected via conveyor belts E1/E2
- Long components exit via Buffer B7

## Input Data
- Excel production plan (one row per beam and per component)
- Includes dimensions, weights, quantities, and processing times for each machine
- MTTF and MTTR data for all machines, analyzed for realistic failure behavior

![image](https://github.com/user-attachments/assets/59e92563-8f36-4c51-8645-83166935ff25)


## Simulation Details

### Model Features
- Reads data dynamically from Excel
- Automates cutting, splitting, batching, and duplication logic
- Uses station-specific flow (R1 to R4) and checks component attributes
- Tracks number of seized resources and throughput per station
- Manages logic for scrap, short parts, and last-piece handling
- Includes buffers, conveyors, cranes, and multiple parallel processes

### Processing Rules
- Entities with `length < 1200mm` and requiring drilling must go to Combined
- Each batch processed according to its `Quantity` and `Profile Type`
- Split logic and attribute assignment drive flow decisions
- Failure distributions applied per machine: 
  - Saw: `Lognormal`
  - Combined: `Weibull`
  - Driller: `Gamma`
  - Crane: `Normal`

## Key Performance Indicators
- **Productivity**: Tons/hour  
- **Machine Utilization**: % time busy per machine  
- **WIP**: Work in progress (tons)  
- **Buffer Saturation**  
- **Processed Units**: Total and per machine

![image](https://github.com/user-attachments/assets/c61b1da7-8241-46eb-970a-5addf8bd03dd)



## Simulation Results Summary
- Average productivity: ~1.92 tons/hour over 5 replications
- R1/R2 were the most utilized conveyors (above 90%)
- Combinated machine showed significantly higher use than saw or drill
- Effective load balancing achieved through dispatching logic and conditional routing
- Higher productivity observed when increasing conveyor speed

![image](https://github.com/user-attachments/assets/760e024b-b0ad-4e70-b472-e11715e95cf3)




