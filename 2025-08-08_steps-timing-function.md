# steps timing function

`steps()` timing function causes an animation to jump from one keyframe to the next without any intermediate frames.

- *step(1, start)*: The animation jumps to the next keyframe at the start of each step.

- *step(1, end)*: The animation jumps to the next keyframe at the end of each step.

- *step(2, start)*: The animation jumps to the next keyframe at the start of every step. There are two steps, so the animation will jump twice.

- *step(2, end)*: The animation jumps to the next keyframe at the end of every step. There are two steps, so the animation will jump twice.

- *step(2, jump-none)*: I have no idea what this does...

- *step(2, jump-both)*: The animation jumps to the next keyframe at both the start and end of each step, So in this case, there are three jumps (start of step 1, end of step 1 = start of step 2, end of step 2). So the animation progress will be divided into three equal parts(0%, 33.33%, 66.66%, 100%).

Below are graphical representations of the different `steps()` timing functions:

![steps-start-n-end.png](../assets/imgs/steps-start-n-end.png)

![steps-none-both.png](../assets/imgs/steps-none-both.png)
