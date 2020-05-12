---
layout: post
title: Optimal Controls MSc Dissertation
date: '2020-12-04'
author: Johan Hentze Svendsen
tags:
- MPC
- LQR
- Bicycle model
- Vehicle Dynamics
- Formula Student
thumbnail_path: blog/Dissertation/MPC.PNG
---
## Introduction:
During my final year in my master’s degree in engineering, I had now found 
a new passion for coding. I felt and still feel that throwing yourself into 
har challenges can help you grow fast. Control theory was something I wanted 
to explore and get experience in, so i sought after a project with this topic. 
I chose to do an optimal control problem for the lateral position of our 
formula student car.

## What could have been better?
Looking back at the project there are some key areas which I wished I did a 
better job, such as in the area of the MPC. Most of the code was done with 
MATLAB specific tools, where my original intent was to code as much as 
possible by myself. But during this time, I decided that the learning I would 
get from writing all of this my myself, was less then what I would get moving 
forward with the MATLAB code. I would also have like to have more testing and 
validation included in my thesis, but as with many theses the hardest part 
is getting the data that you need. We were not many left in the formula student 
team at this moment and therefore time was limited.
Below is an example of some of the data I would have like to revisit, you can 
see that the track has a kink, which is cause the data was done on car data and 
there were cones setup, which had the driver swerving.

{% include figure.html path="projects/trackdata.png" alt="Fault in Track Data" %}

This meant that in the optimisation of trajectory, then the minimum curvature 
curvature calculation got a swerve in the optimisation, which we of course know should 
not be there. But such is it with many dissertations I believe, you acknowledge the areas 
that can be improved, but you will often be limited by time and data.

{% include figure.html path="projects/MCvsSP.png" alt="Minimum Curvature vs Shortest Path" %}

# The good
It was amazing getting to improve my skills in control systems, it is an interesting 
and important part of engineering. Getting both the MPC and LQR working had me 
realise the challenges of running such intensive controllers, which needs powerful 
hardware to compute in real time. 
I enjoyed getting more experience with MATLAB and Simulink, as they are great tools 
for engineers. Later I returned and completed a course on Udemy in MPC's, where I 
got to use python for almost the same task. Here you can see why Simulink is 
quite a powerful tool for control engineers, it is easy to sketch an idea, then 
transfer it to Simulink and start filling out the blocks, even for engineers 
without much experience in coding. I worked with ITK engineering on my project, who 
did a two-day course on controls in Simulink, which really helped kick start my 
project.
Some interesting results were also achieved for Oxford Brookes Racing, who are 
creating the first electric car this year. I got my first experience with the 
concept of hardware in loop and converting Simulink code into code, that can 
be used.
Sometimes I think that not going to the Formula Student competitions would have 
resulted in a better thesis, but I would not have been without the experience I 
got here. I have a desire to someday get a leadership position and at the 
competition I as Race team lead had to organise, lead and make sure we got the 
best results. This experience along with the experience of getting second place 
in one of the toughest competitions in engineering is something I will never 
forget. 

{% include figure.html path="projects/OBR.jpg" alt="Oxford Brookes Racing" %}

# Conclusion
There will also be things that you will want to change when looking back at 
a project, but I believe that taking time to reflect is always good. Sometimes 
just do it to remind yourself of how far you have come. If you look back and 
think I could have done so much better now, that means that you have grown as 
an engineer which is always a good thing.
