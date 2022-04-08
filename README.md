# ICT-project
import pygame
import time
import random
#for creating game needs pygame, also for function of the game need to import time and random
pygame.init()
#set colors coordination and parametr to use in the future 
yellow = (255, 255, 102)
purple= (200, 50, 150)
red = (200, 50, 80)
green = (0, 200, 0)
darkblue = (20, 50, 100)
#This part identify the size of the game 
display_width = 1000
display_height = 800
#this part run the game with parametr width and height, also on the caption write name of the game 
display = pygame.display.set_mode((display_width, display_height))
pygame.display.set_caption('Snake game by 02-P(A)')
clock = pygame.time.Clock()
#identify snake size and speed 
snake_size = 10
snake_velo = 20 
#font settings
font_style = pygame.font.SysFont("Times new roman", 25)
score_font = pygame.font.SysFont("Times new roman", 35) 
#calculate score 
def Score(score):
    value = score_font.render("Score: " + str(score), True, purple)
    display.blit(value, [0, 0]) 
def snake(snake_size, snake_list):
    for a in snake_list:
        pygame.draw.rect(display, green, [a[0], a[1], snake_size, snake_size])
def message(msg, color):
    mesg = font_style.render(msg, True, color)
    display.blit(mesg, [display_width /20 , display_height / 3]) #display_width/4..... needs for arrange message in needed place
 #default parametr in the start of the game, create snake
def gameLoop():
    game_over = False
    game_close = False
    a = display_width / 2
    b = display_height / 2
    a_change = 0
    b_change = 0
    snake_List = []
    Len_snake = 1
 #create food for the snake
    fooda = round(random.randrange(0, display_width - snake_size) / 10.0) * 10.0
    foodb = round(random.randrange(0, display_height - snake_size) / 10.0) * 10.0
    while not game_over:
 #when gamer lose the game
        while game_close == True:
            display.fill(darkblue)
            message("You Lost the game! Press R to Play Again or Q to Quit from the game", red)
            Score(Len_snake - 1)
            pygame.display.update()
 #code for close game or restart
            for event in pygame.event.get():
                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_q:
                        game_over = True
                        game_close = False
                    if event.key == pygame.K_r:
                        gameLoop()
 #respond for snake movements
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    a_change = -snake_size
                    b_change = 0
                elif event.key == pygame.K_RIGHT:
                    a_change = snake_size
                    b_change = 0
                elif event.key == pygame.K_UP:
                    b_change = -snake_size
                    a_change = 0
                elif event.key == pygame.K_DOWN:
                    b_change = snake_size
                    a_change = 0
    #code for,if snake touch boundaries  gamer lose the game  
        if a >= display_width or a < 0 or b >= display_height or b < 0:
            game_close = True
        a += a_change
        b += b_change
        display.fill(darkblue)
        pygame.draw.rect(display, yellow, [fooda, foodb, snake_size, snake_size])
        snake_H = []
        snake_H.append(a)
        snake_H.append(b)
        snake_List.append(snake_H)
        if len(snake_List) > Len_snake:
            del snake_List[0]
        #code for if snake touch yourself, gamer will lose
        for x in snake_List[:-1]:
            if x == snake_H:
                game_close = True
        snake(snake_size, snake_List)
        Score(Len_snake - 1)
        pygame.display.update()
 #code for increase snake size, when snake eat a food
        if a == fooda and b == foodb:
            fooda = round(random.randrange(0, display_width - snake_size) / 10.0) * 10.0
            foodb = round(random.randrange(0, display_height - snake_size) / 10.0) * 10.0
            Len_snake += 1
        clock.tick(snake_velo)
    pygame.quit()
    quit()
gameLoop()
