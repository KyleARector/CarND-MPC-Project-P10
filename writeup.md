## Project 10 - Model Predictive Control
### Project Goals
- Implement a model predictive controller in C++
- Use the MPC to drive a car smoothly around a simulator track
- Determine appropriate equations for state, actuators, and update

---

### Model
For this project, we were given a set of telemetry data from a vehicle in motion, and were to use this data to update the state prediction using a model predictive controller.

The pieces of this telemetry data include:

- Vehicle position in X (px)
- Vehicle position in Y (py)
- Heading of vehicle (psi)
- Vehicle velocity (v)
- Cross track error (cte)
- Vehicle orientation error (epsi)

We are able to control the state with two actuators: throttle and steering angle. Both of these values are output from the MPC, and sent back to the simulation in main.cpp (lines 104-195).

In order to update the state and provide correct actuations, each of the pieces of telemetry data is updated by an equation, and used to evaluate the total cost of the state update. From this process, the set of actuations with the lowest cost is chosen, and performed by the vehicle.

Actions that would drive the car off track, either by turning too hard or driving too quickly, are penalized heavily, making them unattractive choices for the MPC.


### Timestep and Elapsed Duration

The final timestep chosen was 30, and the duration was 0.05. My initial selection was dt = 0.1 and N = 10, as these values more closely correlated with the latency required. However, in testing it was discovered that a duration the same length of the latency lead to unreliable control updates, so a duration half of that was chosen.

The timestep was increased from 10 to 20 initially, in order to give the model a better fitted polynomial. The increase made the model significantly more reliable, and the control much smoother. A further increase to 30 was selected.

It was observed that if the timestep became too large relative to delta t, the polynomial fitting process could result in large path deviations from the ground truth. Longer path predictions could be made with larger timesteps and smaller durations, but this would require more processing power.

### Polynomial Fitting and MPC Preprocessing

The outputs from my MPC are calculated as a third order polynomial, and can be found on lines 97 through 100 in MPC.cpp.

Additionally, the waypoints for the MPC are preprocessed in main.cpp (lines 121-130). This allows a more simple fitting of the polynomial in MPC.cpp. The preprocessing steps include:

- Calculate number of waypoints
- Cycle through each of the waypoints
- Update the waypoints to the vehicle's coordinate system, with the heading of the vehicle being 0 degrees
- Initialize the cross track error, velocity, and orientation error to 0

### Latency

For this project, we were told to assume that the data on the vehicle's state would have a latency of 100 milliseconds. In order to account for this, my code updates the x, y, steering angle, and throttle values. X and Y are updated by adding the product the velocity and the latency value, and then the cosine of psi for X and the sin of psi for Y. The throttle value is updated by adding itself multiplied by the latency. Finally, the steering angle decrements by the product of the latency, velocity, and steering value, all divided by the distance to the car's center of gravity.
