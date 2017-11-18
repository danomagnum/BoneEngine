import Bones
from pygame.locals import *
import pygame
__author__ = 'dano'

WHITE = (255, 255, 255)
BLUE = (0, 0, 255)


def render(skeleton, screen, selected=None):
	for bone in skeleton.bones():
		#p1 = bone.start_point + skeleton.offset
		#p2 = bone.end_point + skeleton.offset
		p1 = bone.start_point
		p2 = bone.end_point
		if bone is selected:
			pygame.draw.line(screen, BLUE, p1.out(), p2.out(), 3)
		else:
			pygame.draw.line(screen, WHITE, p1.out(), p2.out(), 3)
	for orb in skeleton.orbs():
		p1 = orb.start_point
		#p1 = orb.start_point + skeleton.offset
		pygame.draw.circle(screen, WHITE, p1.out(), int(orb.get_length()), 3)
