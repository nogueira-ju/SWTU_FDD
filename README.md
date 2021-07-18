# Fault Detection and Diagnosis in a Sour Water Treatment Unit

This project gathers the databases developed during my masters in Chemical Engineering, in EPQB/UFRJ.

The chemical process of interest is a two-column Sour Water Treatment Unit (SWTU), as shown in the Figure below, to remove H<sub>2</sub>S and NH<sub>3</sub> from industrial sour water.

![dynamicsimulation](https://user-images.githubusercontent.com/87589223/126053770-b4d75a7d-074f-41a7-951b-693a10780030.png)

The units' static and dynamic behavior was simulated using Aspen Plus Dynamics®. The controllers added in the simulation are described in the Table below:

<p align="center">
  <img src="https://user-images.githubusercontent.com/87589223/126053792-c8bf3720-c55c-4f23-9a64-a2fe4ec3df11.PNG">
</p>

Fault-free and faulty scenarios were tested, based on industrial experts advice. A brief description of the classes, including fault-free and simulated faults, is exposed in the following Table, along with its limit values. 

<p align="center">
  <img src="https://user-images.githubusercontent.com/87589223/126053863-df325d37-d305-49f6-90ff-5dd1665479c5.PNG">
</p>

Fault-free simulations are based on small disturbances to stimulate controllers' reactions and create a more diverse database. Those same disturbances were also applied before some fault applications, making a more reality adherent process behavior. There are seven different kinds of disturbances in this work, and they can be defined as small increases and decreases in certain variables, listed as follows: SW1 mass flow, [H<sub>2</sub>S] in SW1, [NH<sub>3</sub>] in SW1, SW1 temperature, SW5 mass flow, CW5 mass flow and AMG1 temperature.

The first three displayed faults are related to the process' feed stream SW1, considering significant variances in its mass flow or composition, to check how the system reacts if changes happen in the inlet stream, simulating typical flooding phenomena that occur in real columns.

Faults 4 and 5 were performed by the insertion of a bias to reduce the pressure on the top of both columns between the columns' exit and the controllers' entrance. Those faults are associated with sensor problems, as the controllers are receiving deceiving pressure values. In a real industrial process, that offset could be caused by clogging.

Although the last tested fault is also related to SW1, the objective is to evaluate how the process reacts to the "overheat" phenomena. As C1's heat duty operational range is typically narrow, small changes could increase gas acid's temperature, reducing separation efficiency. Increasing the process's inlet temperature is a way to simulate that once it would stimulate changes in the column's heat duty.

All the simulations run to generate the database were set with programmable tasks based on scripts to alter the variables automatically, always in an S-ramp shape, using the function "SRAMP" available in the software.

Each of the six faults and fault-free also had several runs, including a fouling effect, as if a heat exchanger had already a certain service period. To represent that, H3's overall heat transfer coefocient, or U-value, was reduced in a single step between 3% and 3.5% at the very beginning of the simulation.

The dynamic data were processed, incluiding noise and delay addition, both intending to make the data more realistc and dynamic, as each sample is composed of two sampling timestamps.

The data were transformed into two databases, train and test, by concatenating the data from multiple simulation runs.
The databases were composed by independent simulation runs so that all the data from each run go to the same database, avoiding leaking information between datasets.
Also, all the classes must be present in both databases, including simulations with and without the fouling effect for each class, so that training is eficient to evaluate the test dataset.

Another concern was class distribution throughout the databases. As the classification performed was based on faults in a chemical process, most of the data should be related to normal operation. The data were standardized to make the AI techniques training process more manageable and less biased.

The train and test databases available in this project are composed of 37519 and 22525 samples, respectively, with about 60% to 65% of fault-free data. The table below displays the distribuition of the datasets:

<p align="center">
  <img src="https://user-images.githubusercontent.com/87589223/126053624-034ee077-09e1-4770-83c5-c44d40b0b90e.PNG">
</p>

The standardized datasets are composed of 27 columns: each of the 13 controlled process variables with delay, that is, in two different sampling times (t) and (t-1), and also the class of the sample. This last entry is an integer that varies from 0 to 6, meaning fault-free and each of the 6 faults studied. 

The standardization was performed in Python 3 using the functions fit and transform from StandardScaler, available in sklearn.preprocessing (scikit-learn 0.23.2).
