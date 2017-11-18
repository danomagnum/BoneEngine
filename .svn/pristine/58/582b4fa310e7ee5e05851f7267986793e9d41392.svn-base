from Bones import *

def sk1():
	a = Bone('base', 300.0, 45)
	b = Bone('offshoot',150.0,0,a,.9)
	c = Bone('offshoot',50.0,90,b,1)
	d = Bone('offshoot',50.0,90,b,0.5)
	e = Bone('offshoot',50.0,90,b,0)
	f = Bone('offshoot',30.0,90,e,0.2)
	g = Bone('offshoot',30.0,90,a,0.5)


	b.theta_max = 90
	b.theta_min = -90

	return Skeleton(a, Point(50, 50))


def guy():
	spine = Bone('spine', 150, 90)

	leftthigh = Bone('leftthigh', 50, 45, spine, 1, 0, 90)

	rightthigh = Bone('rightthigh', 50, -45, spine, 1, -90, 0)

	leftshin = Bone('leftshin', 50, -30, leftthigh, 1, -180, 0)

	rightshin = Bone('rightshin', 50, 30, rightthigh, 1, 0, 180)

	leftbicep = Bone('leftbicep', 50, 45, spine, 0.2, 0, 90)

	rightbicep = Bone('rightbicep', 50, -45, spine, 0.2, -90, 0)

	leftforearm = Bone('leftforearm', 50, -30, leftbicep, 1, -180, 0)

	rightforearm = Bone('rightforearm', 50, 30, rightbicep, 1, 0, 180)

	neck = Bone('neck', 20, -200, spine, 0, -180, -90)

	return Skeleton(spine, Point(150, 150))


def guy2(): # with no bounds on his skeleton
	spine = Bone('spine', 100, 90)

	leftthigh = Bone('leftthigh', 50, 45, spine, 1)

	rightthigh = Bone('rightthigh', 50, -45, spine, 1)

	leftshin = Bone('leftshin', 50, -30, leftthigh, 1)

	rightshin = Bone('rightshin', 50, 30, rightthigh, 1)

	leftbicep = Bone('leftbicep', 50, 45, spine, 0.2)

	rightbicep = Bone('rightbicep', 50, -45, spine, 0.2)

	leftforearm = Bone('leftforearm', 50, -30, leftbicep, 1)

	rightforearm = Bone('rightforearm', 50, 30, rightbicep, 1)

	neck = Bone('neck', 20, -200, spine, 0)

	#foot = Orb('leftfoot',5, leftshin)
	#foot = Orb('rightfoot',5, rightshin)

	#head = Orb('head', 10, neck, 1)
	s = Skeleton(spine, Point(150, 50))

	#s.add_orb('rightforearm', 10)
	#feet
	s.add_orb('leftshin', 8)
	s.add_orb('rightshin', 8)

	return s

def floor():
	start = Bone('a', 100,0)
	next1 = Bone('b',100,15,start)
	next2 = Bone('c',100,-15,next1)
	next3 = Bone('d',100,-15,next2)
	next4 = Bone('e',100,15,next3)
	s = Skeleton(start, Point(0,300))
	return s


def import_position(filename, time, delay):
	f = open(filename)
	commands = []
	for line in f:
		if line != '\n':
			if line[0] != '#':
				commands.append(line + ' ' + str(time) + ' ' + str(delay))
	return commands

def import_commands(filename):
	f = open(filename)
	commands = []
	for line in f:
		if line != '\n':
			if line[0] != '#':
				commands.append(line)
	return commands
