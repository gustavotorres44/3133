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

We want to minize the bank’s total daily labor cost, equal to $160 times the number of full-time workers across the three full-time shifts plus $75 times the number of part-time workers across the two part-time shifts.

$\min \quad 160 \sum_{i=1}^{3} F_i + 75 \sum_{j=1}^{2} P_j$


## Constraints

### 1. Worker Capacity Constraints
The number of check we process each hour ($Z_t$) are limited and cannot exceed the  processing capacity of available workers in each hour.  

$$Z_t \leq 500 \times w_t \quad \text{, ∀ } t = 1, 2, \ldots, 10$$

For convenience, we have created a parameter $w_t$ that represents the different shifts of workers available during hour $t$:
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
The number of checks we process each hour ($Z_t$) are also limited by the 11 processing machines the bank has and the amount of checks that each machine can process. 
The bank has 11 check-processing machines, where each machine processes up to 500 checks. Therefore ($Z_t$) cannot exceed 11 x 500.

$$Z_t \leq 500 \times 11 \quad \text{, ∀ } t = 1, 2, \ldots, 10$$

### 3. Check Flow Balance Constraints
In order to calculate the number of checks we have remaining at the end of each hour $C_t$, we must use the equation below.

$$
C_1 = \text{checks\_arrivals}[0] - Z_1
$$

where $C_1$ is the number of checks remaining at the end of hour 1, 
$\text{checks\_arrivals}[0]$ is the number of checks arriving at the start of hour 1, 
and $Z_1$ is the number of checks processed in hour 1.


$$C_t = \text{checks\_arrivals}[t-1] + C_{t-1} - Z_t \quad \text{, ∀ } t = 2, 3, \ldots, 10$$

Where $\text{checks\_arrivals}[t-1]$ represents the number of new checks arriving at the start of hour $t$.

### 4. End-of-Day Processing Constraint
All checks must be processed by the end of the business day (hour 10):

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
