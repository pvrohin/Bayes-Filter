# Bayes Filter for Robot Navigation

A Python implementation of the Bayes Filter algorithm for state estimation in robotics, specifically designed to estimate the state of a door (open/closed) based on sensor measurements and robot actions.

## Overview

This project demonstrates the implementation of a discrete Bayes Filter for robot state estimation. The filter estimates the probability that a door is open or closed based on:
- **Sensor measurements**: Robot's perception of door state (with measurement noise)
- **Robot actions**: "push" (attempt to open) or "do nothing"
- **System dynamics**: How actions affect the door state

## Problem Description

The robot needs to estimate the state of a door in its environment. The door can be in one of two states:
- **Open** (x = 1)
- **Closed** (x = 0)

### Sensor Model
The robot's sensor has the following characteristics:
- **When door is actually open**: 60% chance of detecting "open", 40% chance of detecting "closed"
- **When door is actually closed**: 20% chance of detecting "open", 80% chance of detecting "closed"

### Action Model
The robot can perform two actions:
- **"push"**: Attempts to open the door
  - If door is already open: stays open (100% probability)
  - If door is closed: 80% chance of opening, 20% chance of staying closed
- **"do nothing"**: No action taken
  - If door is open: stays open (100% probability)
  - If door is closed: stays closed (100% probability)

## Algorithm Implementation

The Bayes Filter follows the standard two-step process:

### 1. Prediction Step
Updates the belief based on the action taken:
```
bel_bar(x_t) = Σ p(x_t | u_t, x_{t-1}) * bel(x_{t-1})
```

### 2. Correction Step
Updates the belief based on the sensor measurement:
```
bel(x_t) = η * p(z_t | x_t) * bel_bar(x_t)
```

Where η is the normalization factor to ensure probabilities sum to 1.

## Files

- `bayes_filter_modified.ipynb`: Main Jupyter notebook containing the complete implementation
- `Bayes Filter.pdf`: Assignment document with problem specifications
- `LICENSE`: MIT License
- `README.md`: This file

## Usage

### Running the Basic Example
```python
# Initialize with unknown door state (50/50 probability)
bel_x0_open = 0.5
bel_x0_closed = 0.5

# Define sequence of actions and measurements
actions = ["do_nothing", "open", "do_nothing", "open", "do_nothing"]
measurements = ["closed", "closed", "closed", "open", "open"]

# Run Bayes filter
for i in range(len(actions)):
    bel_x0_open, bel_x0_closed = bayes_filter(
        actions[i], measurements[i], bel_x0_open, bel_x0_closed
    )
    print(f"Door open probability: {bel_x0_open:.4f}")
```

### Key Functions

- `prediction(action, bel_x0_open, bel_x0_closed)`: Implements the prediction step
- `correction(bel_bar_x1_open, bel_bar_x1_closed, measurement)`: Implements the correction step
- `bayes_filter(action, measurement, bel_x0_open, bel_x0_closed)`: Main filter function

## Results and Analysis

The implementation includes three key experiments:

### Question 1: "Do Nothing" + "Open" Measurement
- **Scenario**: Robot takes no action but consistently measures door as "open"
- **Result**: Takes **9 iterations** to reach 99.99% confidence that door is open
- **Analysis**: Shows how repeated consistent measurements can overcome initial uncertainty

### Question 2: "Open" Action + "Open" Measurement
- **Scenario**: Robot attempts to open door and measures it as "open"
- **Result**: Takes **4 iterations** to reach 99.99% confidence that door is open
- **Analysis**: Demonstrates how actions can accelerate state estimation

### Question 3: "Open" Action + "Closed" Measurement
- **Scenario**: Robot attempts to open door but measures it as "closed"
- **Result**: Takes **10 iterations** to reach steady state
- **Analysis**: Shows convergence behavior when action and measurement are contradictory

## Dependencies

- Python 3.x
- NumPy
- Matplotlib

## Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd Bayes-Filter
```

2. Install required packages:
```bash
pip install numpy matplotlib
```

3. Run the Jupyter notebook:
```bash
jupyter notebook bayes_filter_modified.ipynb
```

## Key Insights

1. **Sensor Reliability**: The sensor is more reliable at detecting closed doors (80% accuracy) than open doors (60% accuracy)

2. **Action Effectiveness**: The "push" action is effective at opening doors (80% success rate) but not guaranteed

3. **Convergence Speed**: Actions that align with measurements lead to faster convergence than passive observation

4. **Steady State**: The filter reaches steady state when belief changes become negligible between iterations

## Educational Value

This implementation serves as an excellent introduction to:
- Probabilistic robotics
- State estimation
- Bayes filtering
- Sensor fusion
- Robot perception

The code is well-commented and includes visualizations to help understand the convergence behavior of the Bayes filter.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Author

Rohin Siddhartha - Advanced Robot Navigation Course Project