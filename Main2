import pygame, random

pygame.init()
width = 800
height = 600
screen = pygame.display.set_mode((width, height))
pygame.display.set_caption("The Dots")

FPS = 60
clock = pygame.time.Clock()

grey =[51, 51, 51]
white = [255, 255, 255]
black = [0, 0, 0]

gameExit = False

class Block():
    def __init__(self, s):
        self.x = self.y = 100
        self.Vx = self.Vy = self.Ax = self.Ay = 0
        self.force = 0.01
        self.s = s

    def update(self):
        self.Ay += self.force
        self.Vx += self.Ax
        self.Vy += self.Ay
        self.x += self.Vx
        self.y += self.Vy

    def show(self):
        pygame.draw.rect(screen, white, [int(self.x), int(self.y), self.s, self.s])
        

square = Block(20)

while not gameExit:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            gameExit = True
            
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_RIGHTARROW:
                pass
    
    screen.fill(grey)


    clock.tick(FPS)
    pygame.display.update()
pygame.quit
