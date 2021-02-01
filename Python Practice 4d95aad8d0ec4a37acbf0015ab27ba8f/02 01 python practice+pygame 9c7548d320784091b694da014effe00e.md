# 02/01 python practice+pygame

1. 함수 한줄로 쓰기 세미콜론(;) 활용

```python
def 서류심사(): print("1.서류심사"); return True
def 인적성검사(): print("2.인적성검사"); return True
def 코딩테스트(): print("3.코딩테스트"); return True
def 면접1차(): print("4.면접1차"); return False
def 면접2차(): print("5.면접2차"); return True
def 신체검사(): print("6.신체검사"); return True

if 서류심사() and 인적성검사() and 코딩테스트() and 면접1차() and 면접2차() and 신체검사():
    print("최종 합격입니다.")
else:
    print("아쉽게도 탈락입니다")
```

2. pygame 전 ddong game 만들기

```python
import random
import pygame
###############################################################################
#기본 초기화 (반드시 해야하는 것들)
pygame.init() #초기화 (반드시 필요)

# 화면 크기 설정
screen_width = 480 # 가로 크기
screen_height = 640 # 세로 크기
screen = pygame.display.set_mode((screen_width, screen_height))

# 화면 타이틀 설정
pygame.display.set_caption("quiz game") # 게임 이름

#FPS
clock = pygame.time.Clock()
###############################################################################

# 1.사용자 게임 초기화 (배경화면, 게임이미지, 좌표, 폰트, 속도 등)
# 배경 만들기
background = pygame.image.load("C:/Users/Hyunsil/Desktop/pythonworkspace/pygame_basic/background.png")

# 캐릭터 만들기
character = pygame.image.load("C:\\Users\\Hyunsil\\Desktop\\pythonworkspace\\pygame_basic\\character.png")
character_size = character.get_rect().size #캐릭터사이즈 이미지의 크기를 구해옴
character_width = character_size[0] # 캐릭터의 가로크기
character_height = character_size[1] # 캐릭터의 세로크기
character_x_pos = (screen_width/2) - (character_width/2) # 화면 가로의 절반 크기에 해당하는 곳에 위치
character_y_pos = screen_height - character_height #화면 세로 크기 가장 아래에 해당하는 곳에 위치

# 똥 만들기
ddong = pygame.image.load("C:\\Users\\Hyunsil\\Desktop\\pythonworkspace\\pygame_basic\\enemy.png")
ddong_size = ddong.get_rect().size
ddong_width = ddong_size[0]
ddong_height = ddong_size[1]
ddong_x_pos = random.randint(0, screen_width - ddong_width)
ddong_y_pos = 0
ddong_speed = 10

#이동할 좌표
to_x = 0
character_speed = 10

running = True # 게임이 진행중인가?
while running:
    dt = clock.tick(30) #게임 화면의 초당 프레임 수를 설정

    # 2. 이벤트 처리 (키보드 마우스 등)
    for event in pygame.event.get(): 
        if event.type == pygame.QUIT: 
            running = False 
    
        if event.type == pygame.KEYDOWN: #키가 눌러졌는지 확인
            if event.key == pygame.K_LEFT: # 캐릭터를 왼쪽으로
                to_x -= character_speed
            elif event.key == pygame.K_RIGHT: #캐릭터를 오른쪽으로
                to_x += character_speed

        if event.type == pygame.KEYUP: #방향키를 떼면 멈춤
            if event.key == pygame.K_LEFT or event.key == pygame.K_RIGHT:
                to_x = 0

    # 3.게임 캐릭터 위치 정의
    character_x_pos += to_x
    if character_x_pos < 0:
        character_x_pos = 0
    elif character_x_pos > screen_width - character_width:
        character_x_pos = screen_width - character_width
    
    ddong_y_pos += ddong_speed

    if ddong_y_pos > screen_height:
        ddong_y_pos = 0
        ddong_x_pos = random.randint(0,screen_width-ddong_width)

    # 4. 충돌 처리
    character_rect = character.get_rect()
    character_rect.left=character_x_pos
    character_rect.top=character_y_pos
    
    ddong_rect = ddong.get_rect()
    ddong_rect.left=ddong_x_pos
    ddong_rect.top=ddong_y_pos

    if character_rect.colliderect(ddong_rect):
        print("충돌했어요")
        running = False

    # 5. 화면에 그리기
    screen.blit(background,(0,0))
    screen.blit(character,(character_x_pos, character_y_pos))
    screen.blit(ddong, (ddong_x_pos, ddong_y_pos))
    pygame.display.update() # 게임화면을 계속 그리게하기

# pygame 종료
pygame.quit()
```