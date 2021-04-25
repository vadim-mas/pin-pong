# pin-pong
from pygame import *

font.init()
font1 = font.Font(None, 36)
font2 = font.Font(None, 80)

class GameSprite(sprite.Sprite):
    def __init__(self, player_image, player_x, player_y, size_x, size_y, player_speed):
        super().__init__()
        self.image = transform.scale(image.load(player_image), (size_x, size_y))
        self.speed = player_speed
        self.rect = self.image.get_rect()
        self.rect.x = player_x
        self.rect.y = player_y

    def reset(self):
        window.blit(self.image, (self.rect.x, self.rect.y))

class Player(GameSprite):
    def update_r(self):
        keys = key.get_pressed()
        if keys[K_UP] and self.rect.y > 5:
            self.rect.y += self.speed
        if keys[K_DOWN] and self.rect.y < win_width - 80:
            self.rect.y -= self.speed

    def update_l(self):
        keys = key.get_pressed()
        if keys[K_w] and self.rect.y > 5:
            self.rect.y += self.speed
        if keys[K_s] and self.rect.y < win_width - 80:
            self.rect.y -= self.speed

game_over = False
lose_l = font2.render('First player loses', True, (180, 0, 0))
lose_r = font2.render('Second player loses', True, (180, 0, 0))

win_width = 700
win_height = 500
img_back = 'ball.jpg'
img_player1 = 'rocket.png'
img_player2 = 'rocket.png'

back = (200, 255, 255)
window = display.set_mode((win_width, win_height))
display.set_caption("Пинг-понг")
background = transform.scale(image.load(img_back), (win_width, win_height))
window.fill(back)
finish = False
game = True
clock = time.Clock()
FPS = 30

rocket1 = Player('rocket.jpg', 30, 200, 4, 50, 150)
rocket2 = Player('rocket.jpg', 520, 200, 4, 50, 150)
ball = GameSprite('ball.jpg', 200, 200, 4, 50, 50)

speed_x = 3
speed_y = 3

while game:
    for e in event.get():
        if e.type == QUIT:
            game = False

        if finish != True:
            window.fill(back)
            rocket1.update_r()
            rocket2.update_l()
            ball.rect.x += speed_x
            ball.rect.y += speed_y

        if sprite.collide_rect(rocket1, ball) or sprite.collide_rect(rocket2, ball):
            speed_x = -speed_x

        if ball.rect.y > win_width - 50 or ball.rect.y < 0:
            speed_y = -speed_y

        if ball.rect.x < win_width:
            finish = True
            window.blit(lose_l, (200, 200))

        if ball.rect.x > win_width:
            finish = True
            window.blit(lose_r, (200, 200))


        rocket1.reset()
        rocket2.reset()
        ball.reset()
    display.update()
    clock.tick(FPS)
    time.delay(50)
