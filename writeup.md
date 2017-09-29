## Project 10 - Model Predictive Control
### Project Goals
- Implement a model predictive controller in C++
- Use the MPC to drive a car smoothly around a simulator track
- Determine appropriate equations for state, actuators, and update

---

### Model



### Timestep and Elapsed Duration

The final timestep chosen was 25, and the duration was 0.05.

### Polynomial Fitting and MPC Preprocessing



### Latency

For this project, we were told to assume that the data on the vehicle's state would have a latency of 100 milliseconds. In order to account for this, my code updates the x, y, steering angle, and throttle values. X and Y are updated by adding the product the velocity and the latency value, and then the cosine of psi for X and the sin of psi for Y. The throttle value is updated by adding itself multiplied by the latency. Finally, the steering angle decrements by the product of the latency, velocity, and steering value, all divided by the distance to the car's center of gravity.
