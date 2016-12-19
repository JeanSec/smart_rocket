import matplotlib.pyplot as plt 
from math import*
import numpy as np 
from random import uniform
from random import randint
from matplotlib import animation
import matplotlib as mpl


lifetime=500
n=50 #individus par génération
class rocket:
	def __init__(self):
		self.n = 6
		self.seq = np.zeros((self.n,lifetime)) #sequence d amorcage
		self.seq[0,:] = [1 for i in range(lifetime)]
		self.dir = [-pi/2,0,0,0,0,0]  #orientation des rockets
		self.v = [0,0]
		self.pos = [100,0]
		self.step = 0  #nombre d etapes
		self.state = 'alive'
		#[nombre de rockets compris entre 1 et 8;
		#sequence de propulsion compris entre 1 et 10;
		#direction de propulsion des rockets]
	def force(self):
		F_result = [0,0]
		for i in range (self.n):
			if self.state == 'alive' and self.seq[i,self.step]>0:
				F_result[0]+=-sin(self.dir[self.seq[i,self.step]-1])
				F_result[1]+=-cos(self.dir[self.seq[i,self.step]-1])
		return F_result

	def mutation(self):
		if randint(0,100)<10:
			self.seq[randint(0,self.n),randint(0,lifetime)] = randint(0,self.n + 1)
		if randint(0,100)<10:
			self.dir[randint(0,5)] = uniform(0,pi)

	def maj_vit(self,Force):
		self.v=[Force[0]+self.v[0],Force[1]+self.v[1]]
	def maj_pos(self):
		self.pos=[self.v[0]+self.pos[0],self.v[1]+self.pos[1]]
	def score(self):
		objectif = [450,450]
		distance = sqrt((self.pos[0]-objectif[0])**2+(self.pos[1]-objectif[1])**2)
		score = 1/(distance+0.01) - self.step
		return score
	def collide(self):
		self.v = [0,0]
		self.state = 'dead'
	

def new_pop(best_element,n):
	new_pop = []
	for i in range(n):
		element = best_element
		element.v = [0,0]
		element.pos = [0,100]
		element = element.mutation()
		new_pop.append(element)
	return new_pop

def step(pop):
	global lifetime,n
	if pop[0].step == (lifetime-1):
		S = 0
		for i in(pop):
			if pop[i].score>S:
				S = pop[i].score
				indice = i
		pop = new_pop(pop[indice],n)
	for i in range(n):
		X=pop[i].force()
		pop[i].maj_vit(X)
		pop[i].maj_pos()
		pop[i].step = pop[i].step + 1
		if pop[i].pos[0]<0 or pop[i].pos[0]>500:
			pop[i].collide()
		elif pop[i].pos[1]<0 or pop[i].pos[0]>500:
			pop[i].collide()
	return pop

population = []
for i in range(n):
	element = rocket()
	population.append(element)




fig, ax = plt.subplots(figsize=(15,15))

ax.set_ylim([0,500])
ax.set_xlim([0,500])


def updatefig(i,fig,pop,scat):
    global n
    step(pop)
    for i in range(n):
        scat.set_offsets(([pop[i].pos[0],pop[i].pos[1]]))
    return scat,

x = [100 for i in range(n)]
y = [0 for i in range(n)]
scat = plt.scatter(x,y,c=x)

anim = animation.FuncAnimation(fig, updatefig, fargs = (fig,population, scat), interval=10, blit=True)

plt.show()
