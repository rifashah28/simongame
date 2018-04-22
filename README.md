# simongame
import pygame
import sys
import random
import musicbox
my_music = musicbox.MusicBox()
dull = pygame.image.load("alldull.jpg")
blue = pygame.image.load("blue.jpg")
red = pygame.image.load("red.jpg")
yellow = pygame.image.load("yellow.jpg")
green = pygame.image.load("green.jpg")


SIMON_NOTES = {1: (green, "E", "52"),
               2: (red, "A", "69"),
               3: (yellow, "C#", "61"),
               4: (blue, "E_lower", "64")
               }

simon_notes = []
simon_midi = []
#Declare the screen size
screen = pygame.display.set_mode((500, 500))

def main():
    win = True
    while win == True:
        simon_midi = simon_combo()
        play_notes(simon_midi)
        color = getColor()
        print(color)
        simon_blit(color)
        user_input = user_combo()
        midi = note_converter(user_input)
        if user_input == 1:
            play_notes(midi)
            screen.blit(green)
            win = compare_combos(midi)

def getColor():
    color = dull
    for i in range(1,5):
        if SIMON_NOTES[i] == 1:
            color = SIMON_NOTES[1][0]
        elif SIMON_NOTES[i] == 2:
            color = SIMON_NOTES[2][0]
        elif SIMON_NOTES[i] == 3:
            color = SIMON_NOTES[3][0]
        elif SIMON_NOTES[i] == 4:
            color = SIMON_NOTES[4][0]


        return color


def play_notes(midi):

    for i in range(len(midi)):
        my_music.play_note(midi[i], 500)


def user_combo():
        x, y = pygame.mouse.get_pos()
        for e in pygame.event.get():
            if e.type == pygame.QUIT: sys.exit()
            elif x >= 150 and x <= 350 and y >= 200 and y <= 300:
                screen.blit(dull, (0, 0))
            elif x >= 0 and x <= 250 and y >= 0 and y <= 250:
                screen.blit(green, (0, 0))
                if e.type == pygame.MOUSEBUTTONDOWN:
                    user_input = 1
                    return user_input
            elif x >= 250 and x <= 500 and y >= 0 and y <= 250:
                screen.blit(red, (0, 0))
                if e.type == pygame.MOUSEBUTTONDOWN:
                    user_input = 2
                    return user_input
                    win = compare_combos(midi, simon_index)
            elif x >= 0 and x <= 250 and y >= 250 and y <= 500:
                screen.blit(yellow, (0, 0))
                if e.type == pygame.MOUSEBUTTONDOWN:
                    user_input = 3
                    return user_input
                    win = compare_combos(midi, simon_index)
            elif x >= 250 and x <= 500 and y >= 250 and y <= 500:
                screen.blit(blue, (0, 0))
                if e.type == pygame.MOUSEBUTTONDOWN:
                    user_input = 4
                    return user_input
        pygame.display.flip()


def simon_combo():
    simon_note = random.randint(1, 4)
    simon_notes.append(simon_note)
    midi = note_converter(simon_note)

    simon_midi.append(midi)
    return simon_midi


def simon_blit(color):
    for i in range(len(simon_notes)):
        buttoninfo = note_converter(i)

        screen.blit(color, (0, 0))


def note_converter(buttonvalue):
    for button in SIMON_NOTES:
        if button == buttonvalue:
            button_data = SIMON_NOTES[button]
            return int(button_data[2].strip('"'))
            # button_info = (button_data[0], button_data[1].strip('"'), int(button_data[2].strip('"')))
            # (value, note, midi) = button_info
            #
            # return midi

def compare_combos(note):
    if simon_midi[-1] == note:
        return True
    else:
        return False

main()
