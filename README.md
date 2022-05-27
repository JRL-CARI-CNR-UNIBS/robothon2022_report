# Robothon 2022 Grand-Challenge CARI Team Report
<p align="center">
Authors: Samuele Sandrini, Cesare Tonola, 
Michele Delledonne, Roberto Pagani
</p>

## Hardware Setup
The robot platform on which the challenge defined by robothon grand challange was tested is shown in the Figure below.
<p align="center">
  <img height="600" src="https://github.com/JRL-CARI-CNR-UNIBS/robothon2022_report/blob/master/images/Robothon_Setup_Without_Electromagnet.png">
</p>

The setup consists of:
- a UR3 six-degree-of-freedom collaborative robot mounted on a fixed table. The robot does not have a force sensor but it estimates it through the current sensor on the joints. 
- a vision system consisting of an rgbd camera and lighting system. Specifically, the camera is a RealSense d435, which was mounted on an articulated mechanical structure maintained statically at the ceiling. Instead, the lighting system was recovered from household applications, in the spirit of competition reuse.
