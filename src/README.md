# CarND-Controls-MPC
Self-Driving Car Engineer Nanodegree Program
Andy Dun
---

## Model and Constraints

The model is the bicycle model described in the lesson, where we approximate the vehicle as a bicycle, with rear wheel that goes straight, and front wheel that steers the front of the car.

The model contains 6 states: x, y, psi, v, cte, and epsi. They represent x, y position, angle of the vehicle, speed of the vehicle, cross track error, and orientation error. In addition, there are two inputs to the model, the steering angle "delta" and throttle input "a". The relationship of the states and the inputs are setup in line 136 to 142 or MPC.cpp. I setup the other constraints between line 103 and 134, which describes the states at different time values.

## Cost Function
The cost to optimize are defined in lines 53 to 93. I tuned the weight of the different costs to achieve a smooth drive around the track. The higher the weight, the more penalty there is to that cost. I found that cte and epsi weights are like P gains in PID, where it reduces rise time but increases vibrations, while change of delta is like a D gain, and reduces vibrations. I also changed the weight for the vehicle speed according to the predicted curvature and change of curvature. This way the vehicle not only slows down for the curve, it also uses the change of the curvature to slow down before the curve, and then accelerate out of it for a more natural feel. The only problem is that the prediction horizon is not that long, and the vehicle slows down quite quickly, and may be uncomfortable.

## Prediction Horizon
Speaking of prediction horizon, I have found that with 3rd degree polynomial, I cannot accurately model multiple turns, so there's no reason to have a super long horizon, plus that adds to the computational cost. So I reduced the horizon to 25 steps and 0.05 second per step. It seems to cover the waypoints provided, and did not slow down the computation much. Other values I have tried are N=10 and dt =0.1. I found that more steps does help the smoothness.

## Polyfit and Preprocessing

I used the polyfit and polyeval provided to fit the waypoints, and to extract cross track error. For preprocessing, I changed the states so that the first and current point is always at position zero with zero angle. 

## Latency

I added the 0.1s latency required in lines 126-135 in main.cpp. It basically predicts the vehicle state in 0.1s based on current trajectory, steering, and acceleration.

## Video
https://youtu.be/L5Fn6TI-g7Y

