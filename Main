import pygame, random, copy
import numpy as np

pygame.init()
width = 800
height = 600
screen = pygame.display.set_mode((width, height))
pygame.display.set_caption("The Dots")

FPS = 0
clock = pygame.time.Clock()
gameExit = False

grey = [30, 30, 30]
white = [255, 255, 255]
black = [0, 0, 0]
red = [255, 0, 0]

goal = [400, 10]




class Dot():
    def __init__(self):
        self.x = int(width/2)
        self.y = int(height - 10)
        self.r = 3
        self.c = white
        self.xVel = self.yVel = 0
        self.xAcc = 0
        self.yAcc = 0
        self.dead = False
        self.steps = 0
        self.reached = False
        self.brain = Brain(400)
        self.fitness = 0
        self.rec = pygame.Rect(int(self.x)-self.r, int(self.y)-self.r, 2*self.r, 2*self.r)
        
    def show(self):
        pygame.draw.circle(screen, self.c, [int(self.x), int(self.y)], self.r)

    def update(self):
        if (self.x >= width or self.x <= 0 or self.y >= height or self.y <= 0):
            self.dead = True
        elif (np.sqrt((self.x-goal[0])**2 + (self.y-goal[1])**2) < 5):
            self.reached = True
        else:    
            for i in obs:
                if self.rec.colliderect(i):
                    self.dead = True
                    
        if not self.dead and not self.reached:
            if len(self.brain.directions) > self.steps:
                self.xAcc = self.brain.directions[self.steps][0]
                self.yAcc = self.brain.directions[self.steps][1]
                self.steps += 1

                self.xVel += self.xAcc
                self.yVel += self.yAcc
                if self.xVel > 5:
                    self.xVel = 5
                if self.yVel > 5:
                    self.yVel = 5
                self.x += self.xVel
                self.y += self.yVel
                self.rec = pygame.Rect(int(self.x)-self.r, int(self.y)-self.r, 2*self.r, 2*self.r)
            else: self.dead = True

    def calculateFitness(self):
        if self.reached:
            self.fitness = 1.0/16.0 + 10000.0/(self.steps * self.steps)
        else:
            distToGoal = np.sqrt((self.x-goal[0])**2 + (self.y-goal[1])**2)
            self.fitness = 1/(distToGoal**2)
        
        return self.fitness

    def getChild(self):
        child = Dot()
        child.brain = copy.deepcopy(self.brain)
        return child

class Brain():
    def __init__(self, size):
        self.size = size
        self.directions = []
        self.randomize()

    def randomize(self):
        self.directions.append((np.random.normal(size=(self.size, 2))).tolist())
        self.directions = self.directions[0]
        
#     def clone(self):
#         clone = Brain(len(self.directions))
#         for i, j in enumerate(self.directions):
#             clone.directions[i] = j
#         return clone

    def mutate(self):
        for i, j in enumerate(self.directions):
            rand = random.random()
            if rand < 0.01:
                self.directions[i] = np.random.normal(size=(1, 2)).tolist()[0]

class Population():
    
    def __init__(self, size):
        self.size = size
        self.dots = []
        self.fitnessSum = 0
        for i in range(self.size):
            self.dots.append(Dot())
            
        self.best = random.choice(self.dots)
        self.bfit = self.best.calculateFitness()

    def show(self):
        for i in self.dots:
            i.show()
        self.best.show()
        
    def onlybest(self):
        self.best.show()

    def update(self):
        self.best.update()
        for i in self.dots:
            i.update()

    def calculateFitness(self):
        for i in self.dots:
            i.calculateFitness()
            if i.fitness > self.bfit:
                self.bfit = i.fitness
                self.best = copy.deepcopy(i)
                
        self.best.c = [255, 255, 0]
        self.best.x = int(width/2)
        self.best.y = int(height - 10)
        self.best.xVel = self.best.yVel = self.best.xAcc = self.best.yAcc = self.best.steps = 0
        self.best.dead = self.best.reached = False
        self.best.rec = pygame.Rect(int(self.best.x)-self.best.r, int(self.best.y)-self.best.r, 2*self.best.r, 2*self.best.r)

    def allDead(self):
        if not self.best.dead and not self.best.reached:
            return False
        for i in self.dots:
            if not i.dead and not i.reached:
                return False
        return True

    def calculateFitnessSum(self):
        self.fitnessSum = 0
        for i in self.dots:
            self.fitnessSum += i.fitness
        self.fitnessSum += self.best.fitness

    def SelectParent(self):
        rand = random.uniform(0, self.fitnessSum)
        runningSum = 0
        for i in self.dots:
            runningSum += i.fitness
            if runningSum > rand:
                return i

    def naturalSelection(self):
        newDots = []
        self.calculateFitnessSum()
        self.dots.append(self.best)
        for i in self.dots:
            parent = self.SelectParent()
            newDots.append(parent.getChild())

        self.dots = newDots[:-1]
        

    def mutate(self):
        for i in self.dots:
            i.brain.mutate()

test = Population(1000)
gen = 1
show = 1
bshow = 0

obs = []
obs.append(pygame.Rect([0, 150, 550, 15]))
obs.append(pygame.Rect([width-225, 150, 225, 15]))

obs.append(pygame.Rect([0, 350, 225, 15]))
obs.append(pygame.Rect([width-550, 350, 550, 15]))
while not gameExit:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            gameExit = True
            
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_s:
                show = (show+1) % 2
            if event.key == pygame.K_b:
                bshow = (bshow+1) % 2
                
    screen.fill(grey)

    if test.allDead():
        #Genetic Algorithm
        test.calculateFitness()
        test.naturalSelection()
        test.mutate()
        gen+=1
        print(gen)
    
    else:
        test.update()
        if bshow:
            test.onlybest()
        elif show:
            test.show()
    if show:
        FPS = 60
        screen.blit(pygame.font.SysFont("comicsansms", 25).render("Gen: "+str(int(gen)), True, white), [20, 540])
        
        pygame.draw.rect(screen, [0, 105, 0], [0, 150, 550, 10])
        pygame.draw.rect(screen, [0, 105, 0], [width, 150, -225, 10])
        
        pygame.draw.rect(screen, [0, 105, 0], [0, 350, 225, 10])
        pygame.draw.rect(screen, [0, 105, 0], [width, 350, -550, 10])
        pygame.draw.circle(screen, red, goal, 4)
        clock.tick(FPS)
        pygame.display.update()
pygame.quit()
