# spatial-modeling-final-project

A description or your project that includes motivation:
The project implements inverse kinematics for a scene where the robot dog always looks
at the ball with a balanced posture. There are two gizmos: one for the ball, and another
one for the fixed location of the robot dog. Once solve_kinematics is pressed in the toolbar,
the solver will set up the described sceneario. The motivation is from the inverse kinematic
application unit from class, since we never done it in our programming assignments before. 
Personally, I think inverse kinematics is a really cool topic as it can realize a scene
with ease under correct mathematical constraints, making the controls more intuitive than
forward kinematics. So I implemented the interfaces and setups for the inverse kinematics,
and drafted up some functions to help me achieve the desired scene.
**NOTE: if after pressing "Solve Inverse Kinematics" the screen freezes, then the optimizer**
**is functioning correctly on the right track. Else press it again, and feel free to reset**
**and re-optimize if the output seems weird.**

The concepts you applied:
It took me a while to realize how inverse kinematic actually works. I originally thought it
works with poses as input and output, but then realized that it takes in DoFs as input
and outputs DoFs, then we run another forward kinematics with these new DoFs to obtain the
poses. The kinematic function concepts include "look-at" controller, parallal to plane, SO3
rotation vector alignment, fixed position, etc. Some are from lecture, and some I invented
myself after some mathematical deduction and discussions with Professor Rakita. I learned that
optimization is much harder than I anticipated. The initial "minimize via negative of max" 
didn't work opposite to what I expected as I naively implemented the function and ran into divergence issue,
so I had to apply Professor Rakita's "minimize towards 0 via subtracting by a threshold" and
"square to ensure smooth, converging function landscape" insights to correct it. It was
a fun trial and error here and there!

If there are any tradeoffs involved in your implementation:
Since the robot's base joint to origin is a floating joint, I decided to use a simple
"match position" constraint to fix the robot to the base gizmo, to avoid tuning further complexity
of "minimizing the distance to the ball while staying on a planar surface with certain feet." Again,
this is a design choice of the scene, and the current scene packed quite a bit of challenge for me
already. Also since solving inverse kinematics via iterative BFGS takes way too long, the demo
becomes laggy if we were to do it at every frame of draw. Therefore, I had to put it outside of the
draw loop so it can only be triggered via the solve_kinematics option. But then the visualization
instantly follows so it's not that much hindering.

If there are any current limitations:
The user has to wait for the robot to fully load in before solving for inverse kinematics, else 
the partial constraints would lead to different and wild results. Also sometimes the solve button
needs to be pressed more than twice to achieve the correct posture, which I find weird and summarize
to "not enough iteration till convergence" which is a hyperparameter choice to tune in the function,
so manually or automatically doesn't matter that much. Lastly, since the implementation of IK
solves for the best current DoFs based on the previous iteration DoFs, there's an interpolation scheme
going on. So if a solve seems wrong, then it's likely that subsequent solves are also wrong. In this
case a reset is needed and a recalculation needs to be done. But once a solve in on the right track,
further solves stay on the right track. Unfortunately, I noticed that sometimes the solve starts off
weirdly in the wrong track, which I summarize to the problem of hitting a different local minimum instead
of a global minimum. So I increased the number of iterations to 1000, and the result became much better.
Just make sure to reset and try optimizing again if something seems weird continuously!
**I'm including a folder of screenshots of successful scenearios, "output examples," to showcase the functional output.**

Overall, it was a fun project, as I went through variety of optimization functions (some failed, some stayed) 
and relentlessly pursued the tiny bits of adjustments (additional small constraints) needed here and there, 
like some constraints to keep the posture going, on top of base code and modifications that set the
foundation of the project.