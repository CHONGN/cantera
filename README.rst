
Nanyang CFD
================

Who we are?
================

We are Nanyang CFD group, which is leaded by `Prof Chan <http://research.ntu.edu.sg/expertise/academicprofile/Pages/StaffProfile.aspx?ST_EMAILID=CHAN.WL&CategoryDescription=Energy>`_. He mainly contributes 
on areas of: 

* Combustion modeling
* Combustion instability
* Computational Fluid Dynamics (CFD)
* Flamelet models
* Fluid-structure interactions
* Gas turbine engines
* High-fidelity simulations
* High-speed air-breathing vehicles
* Large-eddy simulations
* Numerical combustion
* Pollutant emission
* Turbulent non-/reacting flows

What is the special for master cantera?
============

Master cantera, one of modified cantera, was modified by us to 
caculate one-dimentional combustion on Linux. Based on Alejandro M. 
Briones et al.[1], one- and two- point continuation methods[2] have been
merged by us to gain s-curve perfectly, which considers ignition and extinction here.

How to install?
=============

Following us:

1、Install python2.7 and `some other modules according to requirements <https://cantera.org/compiling/installation-reqs.html>`_ in advance
(eg.  g++).

2、Git cantera source code form github directly.
command: git clone https://github.com/CHONGN/master_cantera

2、Install scons using command.

3、`Use scons to install source code of cantera <https://cantera.org/compiling/configure-build.html>`_.

Firstly, move on to your cantera directory through terminal command. eg. command：cd master_cantera/cantera2.4b/.

Then, use scons to build environment through terminal command. eg. command：(sudo) scons build.

Lastly, to install cantera through terminal command. eg. command：(sudo) scons install. 

Examples?
=============
This module here is mainly used in FPV, Flamelet progress variable model.
(eg: ...........)

Reference
=============
[1] `Briones, Alejandro M., et al. "Combustion Modeling Software 
Development, Verification and Validation." ASME 2018 Power Conference
collocated with the ASME 2018 12th International Conference on Energy
Sustainability and the ASME 2018 Nuclear Forum. American Society of 
Mechanical Engineers, 2018 <http://proceedings.asmedigitalcollection.asme.org/proceeding.aspx?articleid=2706214>`_.

[2] `Nishioka, M., C. K. Law, and T. Takeno. "A flame-controlling
continuation method for generating S-curve responses with detailed 
chemistry." Combustion and flame 104.3 (1996): 328-342 <https://www.sciencedirect.com/science/article/abs/pii/0010218095001328>`_.

