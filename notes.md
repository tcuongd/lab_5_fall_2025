# Lab 5 Fall 2025

**What terms are included in your reward functions? What coefficients did you use? How did you come up with these terms and what was their desired effect? Do you think this policy will perform well on the physical robot?**

**Visualize Pupper’s progress during training. How does Pupper look in the first 20 million env steps? How does it look after 200 million env steps?**

Rewards for the most stable policy when deployed on-robot:

```json
{
    "torques": 0,
    "foot_slip": -0.1,
    "lin_vel_z": -0.05,
    "ang_vel_xy": -0.1,
    "action_rate": -0.1,
    "orientation": -0.1,
    "stand_still": 0,
    "termination": 0,
    "feet_air_time": 0.5,
    "body_collision": -0.5,
    "knee_collision": -0.5,
    "abduction_angle": -0.2,
    "mechanical_work": 0,
    "tracking_ang_vel": 1,
    "tracking_lin_vel": 2,
    "joint_acceleration": 0,
    "tracking_orientation": 0,
    "stand_still_joint_velocity": -0.1
}
```

Training params (mostly defaults from the lab):

```json
"ppo": {
    "num_envs": 8192,
    "num_evals": 11,
    "batch_size": 256,
    "discounting": 0.97,
    "entropy_cost": 0.01,
    "action_repeat": 1,
    "learning_rate": 0.00003,
    "num_timesteps": 1000000000,
    "unroll_length": 20,
    "episode_length": 1000,
    "reward_scaling": 1,
    "num_minibatches": 32,
    "num_updates_per_batch": 4,
    "normalize_observations": true
}
```

Eval rollouts throughout training:

![200k steps](./assets/policy10_step200k.mp4)

- Fails to track velocity and acceleration commands. Likely the abduction angle and action rate penalties are dominant here.

![500k steps](./assets/policy10_step500k.mp4)

- Tracks velocity and acceleration commands well, but gait is not natural. Knee angles are not consistent between left and right feet, and it looks like Pupper would struggle to stay balanced.
- It also takes a lot of work to maintain zero velocity; Pupper continually falls to one side and needs to correct.

![1b steps](./assets/policy10_step1b.mp4)

- Follows velocity and acceleration, and gait looks quite natural. Seems to easily maintain zero velocity, although has a slight forward tilt bias that it's constantly correcting for.
- Interestingly, the gait maintains a much lower center of gravity compared to the default RL policy. From real-world deployment, this seems to allow faster movement and turns at the cost of some stability. Pupper is also more vulnerable to tripping on raised surfaces here, as the foot clearance is slightly less compared to default RL policy.








*Record a video of Pupper standing still in simulation. To do this, you can set all commands to zero during visualization. Is Pupper able to stand still given zero commands?*

*In what ways is this policy different on the physical robot (compared to simulation)?*


*Take a video of Pupper walking! Do you notice any differences when Pupper is walking on different surfaces?*

*Inspect Pupper’s gait on each leg, and compare it to the triangle gait from the heuristics walking lab. Do Pupper’s legs move in a similar traingle motion in the gait you discovered? Write a few sentences about the similarities and differences you notice.*

*Comment on what might happen if you add too much domain randomization*

*Report all the graphs you get from the notebook with the best policy. Do a thorough analysis of the graphs: How do the gaits look like for your policy? What are the outputs of the policy you just trained? How do you think it gets passed to Pupper?*
