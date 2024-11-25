A [Mesa](https://github.com/projectmesa/mesa/)-Driven agent-based module for simulating the spreading of an illness between cities in an Italian province 

### Requirements:

- Python 3
- All libraries in requirements.txt
- [Dataset containing the distance matrix of the desired province](https://www.istat.it/non-categorizzato/matrici-di-contiguita-distanza-e-pendolarismo/
)
- [Dataset containing the population of every city in Italy](https://github.com/matteocontrini/comuni-json) (requires JSON to CSV conversion)
- [Dataset containing the position of every city in Italy](https://github.com/MatteoHenryChinaski/Comuni-Italiani-2018-Sql-Json-excel/blob/master/italy_geo.json) (requires JSON to CSV conversion)

### Usage

1) Put the required datasets in a folder called **Data** in the root folder
2) Run the province Jupyter notebook, replacing the province_csv variable with the desired dataset filename
3) Run the main Jupyter notebook, the dashboard will appear in the last cell

![https://elixi.re/i/80dgn.png](https://elixi.re/i/80dgn.png)

### Model Features

- Each city will be assigned 1 agent per 1000 people in the city.
- Each city will be represented with a marker on the map, showing the current number of infected and total agents in the city. The simulation needs to be paused in order to check the markers (thanks Folium!).
- Colors of the markers represent how many agents are in the city. Gray none, Blue between 1 and 5, Orange between 6 and 19, Red for 20 and more.
- At each step, agents will move to a random city with a certain probability. The probability is directly proportional to the population of the destination city (related to the total number of agents) and inversely proportional to the distance between the two cities.
- A certain number of agents will be infected at the start, and they can spread the illness to other agents in the same city. Infected agents can die or recover, after which they cannot be infected again for a certain number of days.
- A vaccination program may also be enabled, affecting all agents not recovering from the illness or already vaccinated.
- Simulation can be started by pressing Run and paused by pressing, you guessed it, Pause. If you want to start the simulation all over, press Run again
- At the end of the simulation (ie, when # of steps reaches the limit or # of infected is 0), the following files will be saved in the **Output/(province)/(timestamp)** folder:
  - **infected.csv**: dataset containing the status of each agent at each step
  - **map.html**: interactive map at the last step of the simulation
  - **plot.png**: plot showing the number of infected, dead and total agents at each step
  - **position.csv**: dataset containing the position (ie the city they're in) of each agent at each step
  - **variables.txt**: text file containing the value of each variable for the specific simulation

### Variables

- **Death Rate**: probability that an infected agents dies
- **Recovery Days**: # of days an agent is infected
- **Antibody Days**: # of days an agent cannot get infected again after being ill
- **Cont. Rate**: probability a random agent in the same city as an infected agent gets infected himself (only if not already infected or recovering)
- **Init Rate**: percentage of agents already infected at step 0
- **Steps**: # of steps
- **Delay**: ms before each step gets calculated
- **Move to Abandoned City**: if True, agents can move to cities with zero agents
- **Open Borders**: if True, agents can move between cities
- **Home Sickness**: if True, after moving to another city, their next move will always be their position at step 0. If Move to Abandoned City is False and the original city of the agent becomes abandoned, they'll be free to move wherever they want at each step
- **Infected Can Move after 1/4 Recovery**: if False, when 1/4 recovery days have passed, an infected agent can no longer move to any city until they recover from their illness.
- **Activate Vaccination**: if True, the following variables will be enabled:
  - **Vaccination after # Steps**: at what step the vaccination process starts
  - **Percentage of Vaccination**: probability that at a given step an agent decides to get vaccinated
  - **Antibody Days Vaccination**: # of days an agent cannot get infected again after being vaccinated