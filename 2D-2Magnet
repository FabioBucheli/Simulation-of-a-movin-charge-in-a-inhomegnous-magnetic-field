from scipy.integrate import solve_ivp
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.pyplot import figure
from matplotlib.widgets import TextBox
from matplotlib.widgets import Button

#Erstellen des Plot-Objekts als 3D Objekt
fig, ax = plt.subplots(figsize=(10, 8))

#Beschriftung der x, y und z Achse

# Definierung der Anfangsposition
x0 = float(0.01)
y0 = float(0.01)
z0 = 0.01

# Definieren der Anfangsgeschwindigkeit
vx0 = 0.0045
vy0 = 0.0055
vz0 = 0.007

# Definieren der Anzahtl Zeitschritte
t0 = 0
tf = 200

# Definieren der Postion und Stärke beider Magneten
m1 = np.array([0, 0.5, 0])*0.001
r1 = np.array([0, 0, 0])
m2 = np.array([0, 0.5, 0])*0.001
r2 = np.array([0.02, 0, 0])

# Einsetzen der Pyhsikalischen variabeln Ladung und Masse
Q = 0.005
a = 0.000000001

# Funtkion zur Verlinkung des Submits zur Integrierten Bewegungsgleichung mit Anpassung der Variablen des Ortes und der Anzahl Zeitschritte.  
def helperX(text):
    global xhelper
    xhelper = float(text)
    global x0 
    x0 = float(text)

def helperY(text):
    global yhelper
    yhelper = float(text)
    global y0 
    y0 = float(text)

def helperZ(text):
    global zhelper
    zhelper = float(text)
    global z0
    z0 = float(text)

def helperVX(text):
    global vxhelper
    vxhelper = float(text)
    global vx0 
    vx0 = float(text)

def helperVY(text):
    global vyhelper
    vyhelper = float(text)
    global vy0 
    vy0 = float(text)

def helperVZ(text):
    global zhelper
    zhelper = float(text)
    global vz0
    vz0 = float(text)

def helperT(text):
    global thelper
    thelper = float(text)
    # Befehl zur Ausführung der Simulation bei einem Submit. Ein Submit passiert bei einem Druck der Enter-Taste oder beim Herauklicken aus dem Kasten.
    submit(x0, y0, z0, vx0, vy0, vz0, float(text))
    global tf
    tf = float(text)

# Funktion zur Berechnung der Bewegung des Teilchens und Defintion der Parameter, die im Plot angezeigt werden.
def submit(xx, yy, zz, vxx, vyy, vzz, tf):

    print(xx, yy)

    def mag_bewegung(t, D, m1, m2, Q, r1, r2, a):
        # Definition von der linken Seite der veränderlichen Parameter in der Differntialgleichung. (linke Seite)
        x, y, z, vx, vy, vz = D
        # Festlegung des Ortes und der Geschwindigkeit als array.
        v = np.array([vx, vy, vz])
        o = np.array([x, y, z])
        # Berechnung des Abstandes von der bewegten Ladung zu den beiden Magneten.
        ra1 = o - r1
        ra2 = o - r2
        # Berechnung des Betrags des Abstandes von der bewegten Ladung zu den beiden Magneten.
        rar1 = np.sqrt(sum(ra1*ra1))
        rar2 = np.sqrt(sum(ra2*ra2))
        # Berechnung der beiden Magnetflüsse am Ort des Teilchens, welche von den beiden Stabmagneten ausgeht. Diese Gleichung rechnet mit dem Abstand von den Magneten zu dem Körper ra1,ra2 und dessen Beträge rar1,rar2 und Dipolmoment der Magnete m1,m2
        B1 = 0.00000001*((3 * (np.dot(m1, (ra1)/rar1))*((ra1)/rar1)-m1)/(rar1*rar1*rar1))
        B2 = 0.00000001*((3 * (np.dot(m2, (ra2)/rar2))*((ra2)/rar2)-m2)/(rar2*rar2*rar2))
        # Berechnung der Summe der Magnetflüsse
        B = B1 + B2
        # Berechnung der Beschleunigung, die die bewegte Ladung erfährt.
        s = (Q*np.cross(v, B))/a
        # Aufspaltung des Array für Beschleunigung in x, y, z für die Differentialgleichung.
        sx = s[0]
        sy = s[1]
        sz = s[2]
        # Festlegung der Differentialgleichung
        fun = [vx, vy, vz, sx, sy, sz]
        return fun

    # Festlegung der Variabeln für die Startbedingungen
    D0 = [xx, yy, zz, vxx, vyy, vzz]

    # Methode für Lösung des Startwertproblems mithilfe von RungaKutta des 8ten Grades
    NumSol = solve_ivp(mag_bewegung, [t0, tf], D0, method="DOP853", args=(m1, m2, Q, r1, r2, a), atol=1e-10, rtol=1e-13)
    # Aufspaltung der Geschwindigkeit und des Ortes des Teilchens und des Zeitschrittes aus dem Objekt, das aus der solve_ivp Methode kommt. Dies erhält man als array und 
    aa, bb, cc, dd, ee, ff = NumSol.y
    t = NumSol.t
    
    # Definieren welche Werte im Plot auf welchen Achsen angezeigt werden
    ax.plot(cc, t)
    plt.draw()

#Definierung des Ortes und Grösse des Eingabefeldes
xbox = plt.axes([0.05, 0.1125, 0.125, 0.05])
ybox = plt.axes([0.05, 0.0625, 0.125, 0.05])
zbox = plt.axes([0.05, 0.0125, 0.125, 0.05])

vxbox = plt.axes([0.05, 0.275, 0.125, 0.05])
vybox = plt.axes([0.05, 0.225, 0.125, 0.05])
vzbox = plt.axes([0.05, 0.175, 0.125, 0.05])

tbox = plt.axes([0.05, 0.3325, 0.125, 0.05])

#Definierung des Textbox Objekt des Eingabefeldes mit der obig Definierten Ort und Grösse.
KoordinateX = TextBox(xbox, 'X0', initial=float(x0))
KoordinateY = TextBox(ybox, 'Y0', initial=float(y0))
KoordinateZ = TextBox(zbox, 'Z0', initial=float(z0))

KoordinateVX = TextBox(vxbox, 'VX0', initial=float(vx0))
KoordinateVY = TextBox(vybox, 'VY0', initial=float(vy0))
KoordinateVZ = TextBox(vzbox, 'VZ0', initial=float(vz0))

ZeitT = TextBox(tbox, 'T0', initial=float(tf))

#Trigger zu Auslöung der helper-Funktion bei einer Eingabe in das Textfeld
KoordinateX.on_submit(helperX)
KoordinateY.on_submit(helperY)
KoordinateZ.on_submit(helperZ)

KoordinateVX.on_submit(helperVX)
KoordinateVY.on_submit(helperVY)
KoordinateVZ.on_submit(helperVZ)

ZeitT.on_submit(helperT)

#Befehl zur Anzeige des Plots
plt.show()
