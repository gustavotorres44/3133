# ISYE 3133 PA1 Report

## Problem Overview

- Bank One  receives checks each hour from 10am to 7pm (10 hours). 
- It must process all checks the same day received.
- There are 11 check processing machines, capable of processing up to 500 checks / hour. 
- Each machine requires 1 worker to operate it.
- The bank requires at least 3 Full Time workers to be hired at all times.

Bank One employs two types of workers:
- **Full-time employees** ($160/day): Work one of three 8-hour shifts (10am-6pm, 11am-7pm, or 12pm-8pm)
- **Part-time employees** ($75/day): Work one of two 5-hour shifts (2pm-7pm or 3pm-8pm)


## Section 1: Linear Program Optimizing Bank One Operations

## Decision Variables

We have designated DVs for the number of workers assigned to each shift, for both full time and part time workers.

**Worker Decision Variables:**
- $F_i$ = Number of full-time workers assigned to shift $i$, ∀ $i = 1, 2, 3$
  - $F_1$: Full-time workers working 10am-6pm shift
  - $F_2$: Full-time workers working 11am-7pm shift
  - $F_3$: Full-time workers working 12pm-8pm shift

- $P_j$ = Number of part-time workers assigned to shift $j$, ∀ $j = 1, 2$
  - $P_1$: Part-time workers working 2pm-7pm shift
  - $P_2$: Part-time workers working 3pm-8pm shift

It is also required that we create a decision variable for the number of checks that we can process in each hour. 

**Check Processing Decision Variables:**
- $Z_t$ = Number of checks processed during hour $t$, ∀ $t = 1, 2, \ldots, 10$
  - Hour 1 corresponds to 10am-11am, Hour 2 to 11am-12pm, etc.


We are also required to create a decision variable for the number of checks that remain unprocessed at the end of each hour. This information is necessary to be able to optimize the number of workers for each hour correctly. 

**Inventory Variables:**
- $C_t$ = Number of checks remaining unprocessed at the end of hour $t$, ∀ $t = 1, 2, \ldots, 10$


## Objective Function

**Minimize** the banks total labor costs. 

$$\text{Minimize } Z = 160(F_1 + F_2 + F_3) + 75(P_1 + P_2)$$

Where:
- $160 represents the daily wage for each full-time worker
- $75 represents the daily wage for each part-time worker

## Constraints

### 1. Worker Capacity Constraints
The number of checks processed each hour cannot exceed the processing capacity of available workers:

$$Z_t \leq 500 \times w_t \quad \text{, ∀ } t = 1, 2, \ldots, 10$$

Where $w_t$ represents the number of workers available during hour $t$:
- $w_1 = F_1$ (10am-11am)
- $w_2 = F_1 + F_2$ (11am-12pm)
- $w_3 = F_1 + F_2 + F_3$ (12pm-1pm)
- $w_4 = F_1 + F_2 + F_3$ (1pm-2pm)
- $w_5 = F_1 + F_2 + F_3 + P_1$ (2pm-3pm)
- $w_6 = F_1 + F_2 + F_3 + P_1 + P_2$ (3pm-4pm)
- $w_7 = F_1 + F_2 + F_3 + P_1 + P_2$ (4pm-5pm)
- $w_8 = F_1 + F_2 + F_3 + P_1 + P_2$ (5pm-6pm)
- $w_9 = F_2 + F_3 + P_1 + P_2$ (6pm-7pm)
- $w_{10} = F_3 + P_2$ (7pm-8pm)

### 2. Machine Capacity Constraints
The bank has 11 check-processing machines, limiting total hourly processing:

$$Z_t \leq 500 \times 11 \quad \text{, ∀ } t = 1, 2, \ldots, 10$$

### 3. Check Flow Balance Constraints
The number of remaining checks follows the flow balance equation:

$$C_1 = \text{checks\_arrivals}[0] - Z_1$$

$$C_t = \text{checks\_arrivals}[t-1] + C_{t-1} - Z_t \quad \text{, ∀ } t = 2, 3, \ldots, 10$$

Where $\text{checks\_arrivals}[t-1]$ represents the number of new checks arriving at the start of hour $t$.

### 4. End-of-Day Processing Constraint
All checks must be processed by the end of the business day:

$$C_{10} \leq 0$$

### 5. Minimum Full-Time Worker Constraint
Bank One requires at least 3 full-time workers for operational continuity:

$$F_1 + F_2 + F_3 \geq 3$$

### 6. Non-Negativity Constraints
All decision variables must be non-negative:

$$F_i \geq 0 \quad \text{, ∀ } i = 1, 2, 3$$
$$P_j \geq 0 \quad \text{, ∀ } j = 1, 2$$
$$Z_t \geq 0 \quad \text{, ∀ } t = 1, 2, \ldots, 10$$
$$C_t \geq 0 \quad \text{, ∀ } t = 1, 2, \ldots, 10$$

## Model Scalability

This linear programming formulation is designed to be robust and scalable:

- **Data Updates:** The model can handle changes in check arrival patterns by updating the `checks_arrivals` array
- **Capacity Changes:** Machine limitations can be modified by changing the coefficient 11 in the machine capacity constraints
- **Wage Changes:** Worker costs can be adjusted by modifying the coefficients 160 and 75 in the objective function
- **Shift Changes:** Worker availability patterns can be modified by adjusting the $w_t$ definitions
- **Processing Efficiency:** The 500 checks/hour rate can be updated throughout all relevant constraints

This comprehensive model ensures that Bank One can minimize labor costs while meeting all operational requirements and processing all checks by the end of each business day.
