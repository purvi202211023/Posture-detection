# Posture-detection
Poor posture is a modern-day epidemic. Research has shown that children as young as 10 years of age are demonstrating spinal degeneration on x-ray. Certain postures, like forward head posture, have been linked to migraines, high blood pressure, and decreased lung capacity. In this notebook, you will learn to create an application to detect posture using mediapipe. We are going to use side view samples to perform analysis and draw conclusion. Practical application would require an webcam pointed at the side view of the person.

### Workflow
 - Find point of interests (landmarks) to check angle.
 - Find threshold range for good and bad posture.
 - Apply on video/webcam input.
 
### Goal
To detect neck inclination and torso inclination.

## Function to calculate offset distance
The setup requires the person to be in proper side view. **`findDistance`** function helps us determine offset distance between two poinst. It can be the hip points, the eyes or shoulder. As these points are always more or less symmetric about the central axis. With this, we are going to incorporate camera alignment assistance in the script. Distnace is calculated using the distance formula.


\begin{align}
distance =  \sqrt{(x2 - x1)^2+(y2 - y1)^2}
\end{align}

## Function to calculate angle subtended by the line of interest to y-axis
This is the primary deterministic factor for the posture. Using the angle subtended by the neck line and the torso line to y-axis. The neck line connects the shoulder and the eye, here we take shoulder as the pivotal point. Similarly the torso line connects hip and the shoulder, where hip is considered pivotal point. 
<br>

<img src="https://learnopencv.com/wp-content/uploads/2022/03/mp-pose-05-neckline-inclination.jpg" alt="MediaPipe pose neck inclination" align="middle" width="300" height="250">

<br>

Taking neck line as an example, here points are $P_1(x_1, y_1)$(shoulder), $P_2(x_2, y_2)$ (eye) and $P_3(x_3, y_3)$ (any point on vertical axis passing through $P_1$). 
<br>
Hence, for $P_3$ x-coordinate is same as to that of $P_1$ and since $y_3$ is valid for all y, let's take $y_3 = 0$ for simplicity. <br>To find the inner angle of three points, we take vector approach. Angle between two vectors $\vec{P_{12}}$ and $\vec{P_{13}}$ is given by,
\begin{align}
\theta = \arccos (\frac{\vec{P_{12}}.\vec{P_{13}}}{|\vec{P_{12}}|.|\vec{P_{13}}|})
\end{align}
Solving for $\theta$ we get, 
\begin{align}
\theta = \arccos (\frac{y_1^2 - y_1.y_2}{y_1\sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2}})
\end{align}

