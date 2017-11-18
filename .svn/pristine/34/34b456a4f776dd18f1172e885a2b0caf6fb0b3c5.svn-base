import Bones
import Skeletons
import Renderer
from pygame.locals import *
import pygame
import pygame.font
import time
__author__ = 'dano'
BLACK = (0,0,0)
WHITE = (255,255,255)


sk1 = Skeletons.guy2()
#sk2 = Skeletons.guy2()
#sk2.translate(120,0)
floor = Skeletons.floor()

screen = pygame.display.set_mode((640, 480))
clock = pygame.time.Clock()  # start the clock
pygame.init()
font = pygame.font.Font(None, 20)
desc = 'A: reset boxer, S: punch, D: disable automovement, f: kick'
dtext = font.render(desc, True, (255, 255, 255))


running = 1
selected_bone = None
dumpedtext = ''

boxercommands = Skeletons.import_position('Positions/boxer', 20, 0)
#punchcommand = Skeletons.import_position('Positions/punch', 10, 0)
#punchcommand += Skeletons.import_position('Positions/boxer', 20, 10)
punchcommand = Skeletons.import_commands('Moves/punch')
kick = Skeletons.import_commands('Moves/kick2')
jump = Skeletons.import_commands('Moves/jump')
crouch = Skeletons.import_commands('Moves/crouch')
idle = Skeletons.import_commands('Moves/idle2')
walk = Skeletons.import_commands('Moves/walk')
enable_idle = True
enable_gravity = True
facing_right = True
walking = False
sk1.parse_commands(boxercommands)
on_ground = False
crouching = False

while running:
	for event in pygame.event.get():
		if event.type == pygame.QUIT:
			running = False
		elif event.type == pygame.MOUSEBUTTONDOWN:
			if event.button == 4:  # scroll wheel up
				if selected_bone is not None:
					selected_bone.rotate(1)
					selected_bone.calculate_position()
			elif event.button == 5:  # scroll wheel up
				if selected_bone is not None:
					selected_bone.rotate(-1)
					selected_bone.calculate_position()
			else:
				x0,y0 = pygame.mouse.get_pos()
				pt = Bones.Point(x0, y0)
				for bone in sk1.bones() + floor.bones():
					if bone.intersects_point(pt, 4):
						selected_bone = bone
						print 'selected ', bone.name
						break
				else: #this is that qwirky else on a for loop where it happens if you don't break.
					selected_bone = None
		elif event.type == pygame.KEYDOWN:
			if event.key == pygame.K_RETURN:
				dumpedtext += sk1.dump_position()
				print '------------'
				print sk1.dump_position()
				print '------------'
			elif event.key == pygame.K_a:
				for line in boxercommands:
					sk1.parse_command(line)
			elif event.key == pygame.K_s:
				for line in punchcommand:
					sk1.parse_command(line)
			elif event.key == pygame.K_f:
				sk1.parse_commands(kick)
			elif event.key == pygame.K_d:
				enable_idle = not enable_idle
				enable_gravity = not enable_gravity
			elif event.key == pygame.K_RIGHT:
				if facing_right:
					sk1.parse_commands(walk)
				else:
					facing_right = True
					sk1.mirrorLR = not sk1.mirrorLR
					sk1.parse_commands(walk)
				walking = True
			elif event.key == pygame.K_LEFT:
				if facing_right:
					facing_right = False
					sk1.mirrorLR = not sk1.mirrorLR
					sk1.parse_commands(walk)
				else:
					sk1.parse_commands(walk)
				walking = True
			elif event.key == pygame.K_UP:
				sk1.parse_commands(jump)
			elif event.key == pygame.K_DOWN:
				crouching = True
				sk1.parse_commands(crouch)

		elif event.type == pygame.KEYUP:
			if event.key == pygame.K_LEFT or event.key == pygame.K_RIGHT:
				walking = False
			if event.key == pygame.K_DOWN:
				crouching = False



	if sk1.has_commands():
		sk1.execute()
	else:
		if walking:
			sk1.parse_commands(walk)
		elif crouching:
			if enable_idle:
				sk1.parse_commands(crouch)
				sk1.parse_commands(idle)
		else:
			if enable_idle:
				sk1.parse_commands(boxercommands)
				sk1.parse_commands(idle)


	if enable_gravity:
		if not on_ground:
			#falling
			sk1.translate(0,1)

	screen.fill(BLACK)
	screen.blit(dtext, (0,0))
	Renderer.render(sk1, screen, selected_bone)
	Renderer.render(floor, screen, selected_bone)
	#Renderer.render(sk2, screen)

	x0,y0 = pygame.mouse.get_pos()
	for orb in sk1.orbs():
		if orb.intersects_point(Bones.Point(x0, y0)):
			#print 'Hit mouse', time.time()
			continue

	if enable_gravity:
		on_ground = False
		for floorbone in floor.bones():
			for orb in sk1.orbs():
				if orb.intersects_line(floorbone):
					on_ground = True
					break
			for bone in sk1.bones():
				if bone.intersects_line(floorbone):
					on_ground = True
					sk1.translate(0,-3)



	pygame.display.flip()
	clock.tick(60)
	pygame.display.set_caption("BoneTest (%.2f fps)" % (clock.get_fps()))  # show the FPS in the title bar
