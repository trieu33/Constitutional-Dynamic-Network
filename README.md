# Constitutional-Dynamic-Network
CDNs projects
### Thermodynamic Model for Constitutional Dynamic Networks adaptation to micellar micro-compartmentalization
from scipy.optimize import fsolve
import math
import matplotlib.pyplot as plt
import ipywidgets as widgets
from ipywidgets import interact, interactive, fixed, interact_manual
### We want to determine the composition of the system: 4 components and 4 constituents
### We therefore need 8 equations to solve the system
### The connectivities are simply included on the equilibrium constants in presence of 250 mM SDS determined by 1H NMR such as:
### KA4B4=[A4B4]/[A4]*[B4]=0.14 
### KA4B2=[A4B2]/[A4]*[B2]=1.29
### KA5B4=[A5B4]/[A5]*[B4]=0.83
### KA4B4=[A5B2]/[A5]*[B2]=0.39
### Conservation of matter provides four more equations from initial conditions:
### [A5]+[A5B4]+[A5B2]=10
### [A4]+[A4B4]+[A4B2]=10
### [B4]+[A5B4]+[A4B4]=10
### [B2]+[A5B2]+[A4B2]=10
def update(a = widgets.FloatSlider(min=0, max=10, step=0.01, value=0.83), 
           b = widgets.FloatSlider(min=0, max=10, step=0.01, value=0.39), 
           c = widgets.FloatSlider(min=0, max=10, step=0.01, value=0.14), 
           d = widgets.FloatSlider(min=0, max=10, step=0.01, value=1.29)):
    def equations(p):
        a5, a4, b4, b2, a5b4, a5b2, a4b4, a4b2 = p
        return (a5b4/a5/b4 - a, a5b2/a5/b2 - b, a4b4/a4/b4 - c, 
                a4b2/a4/b2 - d, a5 + a5b2 + a5b4 - 10, a4 + a4b2 + a4b4 - 10,
                b4 + a4b4 + a5b4 - 10, b2 + a5b2 + a4b2 - 10)
### Transcription of the network's connectivities into thermodynamic equations provides an accessible and reliable prediction of the system redistribution
    a5, a4, b4, b2, a5b4, a5b2, a4b4, a4b2 = fsolve(equations, 
                                                       (1, 1, 1, 1, 1, 1 ,1, 1))
    
    sums = (a5 + a4 + a5b4 + a5b2 + a4b4 + a4b2) / 100
 
    print("A4B2  : %.2f\nA4B4  : %.2f" % (a4b2/sums, a4b4/sums))
    print("A5B2 : %.2f\nA5B4 : %.2f" % (a5b2/sums, a5b4/sums))
    print("A4    : %.2f\nA5   : %.2f" % (a4/sums, a5/sums))
### We can therefore play with the equilibrium constants to define the requirements for the described implementation of micro-compartmentalization features into constitutional dynamic networks.
    fig = plt.figure()
    ax = fig.add_subplot(1, 1, 1)
    compounds = ['A4B2', 'A4B4', 'A5B2', 'A5B4', 'A4', 'A5']
    percs = [a4b2/sums, a4b4/sums, a5b2/sums, a5b4/sums, a4/sums, a5/sums]
    colors = ['green', 'blue', 'red', 'orange', 'pink', 'grey']
    ax.bar(compounds,percs,color=colors)

    plt.ylabel("Composition [%]")
    fig.canvas.draw_idle()

interact(update);
