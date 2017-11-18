import math
__author__ = 'dano'

SIGMA = 0.1



def orientation(p, q, r):
	v = (q.y - p.y) * (r.x - q.x) - (q.x - p.x) * (r.y - q.y)
	if v == 0:
		return 0
	if v > 0:
		return 1
	else:
		return 2

def on_segment(p, q, r):
	if (q.x <= max(p.x, r.x)) and (q.x >= min(p.x, r.x)) and (q.y <= max(p.y, r.y)) and (q.y >= p.y, r.y):
		return True
	else:
		return False

class Point(object):
	def __init__(self, x, y):
		self.x = x
		self.y = y
	def out(self):
		return int(self.x),int(self.y)
	def __add__(self, other):
		return Point(self.x + other.x, self.y + other.y)
	def __sub__(self, other):
		return Point(self.x - other.x, self.y - other.y)
	def distance(self, other):
		return math.sqrt((self.x - other.x)**2 + (self.y - other.y)**2)



class NOP(object):
	def __init__(self, *args):
		pass
	def tick(self, *args):
		return False

class BoneRotateRelative(object):
	def __init__(self, bonename, degrees, ticks, delay = 0):
		self.bonename = str(bonename)
		self.degrees = float(degrees)
		self.ticks = int(ticks)
		self.delay = int(delay)
		self.delta = None

	def tick(self, skeleton):
		if self.delay > 0:
			self.delay -= 1
			return True
		else:
			bone = skeleton.bonenames.get(self.bonename)
			if bone is not None:
				if self.delta is None:
					self.delta = float(self.degrees) / float(self.ticks)
				bone.rotate(self.delta)
				self.degrees -= self.delta
				self.ticks -= 1
				if self.ticks > 0:
					return True
				else:
					return False
			else:
				print "couldn't find bone", self.bonename
				return False

class BoneRotateAbsolute(object):
	def __init__(self, bonename, degrees, ticks, delay = 0):
		self.bonename = str(bonename)
		self.degrees = float(degrees)
		self.ticks = int(ticks)
		self.delay = int(delay)
		self.delta = None
		self.cw = True

	def tick(self, skeleton):
		if self.delay > 0:
			self.delay -= 1
			return True
		else:
			bone = skeleton.bonenames.get(self.bonename)
			if self.delta is None:
				if self.degrees > bone.theta:
					self.delta = (self.degrees - bone.theta )/float(self.ticks)
					self.cw = True
				else:
					self.delta = (bone.theta - self.degrees )/float(self.ticks)
					self.cw = False

			if self.cw:
				bone.rotate_absolute(bone.theta + self.delta)
			else:
				bone.rotate_absolute(bone.theta - self.delta)

			self.ticks -= 1
			if self.ticks > 0:
				return True
			else:
				return False

class BoneRotateAbsoluteCW(object):
	def __init__(self, bonename, degrees, ticks, delay = 0):
		self.bonename = str(bonename)
		self.degrees = float(degrees)
		self.ticks = int(ticks)
		self.delay = int(delay)
		self.delta = None

	def tick(self, skeleton):
		if self.delay > 0:
			self.delay -= 1
			return True
		else:
			bone = skeleton.bonenames.get(self.bonename)
			if self.delta is None:
				self.delta = (self.degrees - bone.theta )/float(self.ticks)
			bone.rotate_absolute(bone.theta + self.delta)
			self.ticks -= 1
			if self.ticks > 0:
				return True
			else:
				return False

class BoneRotateAbsoluteCCW(object):
	def __init__(self, bonename, degrees, ticks, delay = 0):
		self.bonename = str(bonename)
		self.degrees = float(degrees)
		self.ticks = int(ticks)
		self.delay = int(delay)
		self.delta = None

	def tick(self, skeleton):
		if self.delay > 0:
			self.delay -= 1
			return True
		else:
			bone = skeleton.bonenames.get(self.bonename)
			if self.delta is None:
				self.delta = (bone.theta - self.degrees )/float(self.ticks)
			bone.rotate_absolute(bone.theta - self.delta)
			self.ticks -= 1
			if self.ticks > 0:
				return True
			else:
				return False

class SkeletonTranslateRelative(object):
	def __init__(self, dx, dy, ticks, delay = 0):
		self.target_dx = int(dx)
		self.target_dy = int(dy)
		self.ticks = int(ticks)
		self.delay = int(delay)
		self.dx = None
		self.dy = None

	def __str__(self):
		return 'STR ' + str(self.target_dx) + ', ' + self.target_dy

	def tick(self, skeleton):
		if self.delay > 0:
			self.delay -= 1
			return True
		else:
			if self.dx is None:
				self.dx = float(self.target_dx) / float(self.ticks)
				self.dy = float(self.target_dy) / float(self.ticks)
			skeleton.translate(self.dx, self.dy )
			self.ticks -= 1
			if self.ticks > 0:
				return True
			else:
				return False

class SkeletonScaleAbsolute(object):
	def __init__(self, scale, ticks, delay = 0):
		self.target = float(scale)
		self.ticks = int(ticks)
		self.delay = int(delay)
		self.delta = None

	def tick(self, skeleton):
		if self.delay > 0:
			self.delay -= 1
			return True
		else:
			if self.delta is None:
				self.delta = (self.target - skeleton.current_scale)/self.ticks
			skeleton.scale(skeleton.current_scale + self.delta)
			self.ticks -= 1
			if self.ticks > 0:
				return True
			else:
				return False

class AddOrb(object):
	def __init__(self, bonename, radius, position, ticks, delay):
		self.bonename = str(bonename)
		self.radius = float(radius)
		self.position = float(position)
		self.ticks = int(ticks)
		self.delay = int(delay)
		self.orb = None

	def tick(self, skeleton):
		if self.delay > 0:
			self.delay -= 1
			return True
		else:
			if not self.orb:
				self.orb = skeleton.add_orb(self.bonename, self.radius, self.position)
			self.ticks -= 1
			if self.ticks > 0:
				return True
			else:
				skeleton.remove_orb(self.orb.name)
				return False

COMMANDS = {'BRR':BoneRotateRelative,
		    'BRA':BoneRotateAbsolute,
		    'BRACW': BoneRotateAbsoluteCW,
		    'BRACCW': BoneRotateAbsoluteCCW,
		    'STR': SkeletonTranslateRelative,
		    'SSA': SkeletonScaleAbsolute,
            'AO': AddOrb,
            'NOP': NOP
}

class Skeleton(object):
	def __init__(self, root_bone, offset, scale=1.0):
		self.root = root_bone
		self.offset = offset
		self.bonenames = {}
		self.orbnames = {}
		self.commands = []
		self.current_scale = scale
		self.orbid = 0
		for bone in self.bones():
			self.bonenames[bone.name] = bone
			bone.skeleton = self

		self.mirrorLR = False

		self.root.calculate_position()

	def add_orb(self, bonename, radius, percent=1):
		o = Orb(str(self.orbid), radius, self.bonenames[bonename], percent, self.current_scale)
		self.orbid += 1
		self.orbnames[o.name] = o
		return o

	def remove_orb(self, orbid):
		del(self.orbnames[orbid])

	def translate(self, x, y):
		if self.mirrorLR:
			self.offset += Point(-1 * x, y)
		else:
			self.offset += Point(x, y)

	def scale(self, new_scale):
		self.current_scale = new_scale
		for bone in self.bones():
			bone.scale = new_scale

	def orbs(self):
		return self.orbnames.values()

	def bones(self):
		return self.root.get_all_children()

	def parse_commands(self, commandlist):
		for cmd in commandlist:
			self.parse_command(cmd)

	def parse_command(self, command):
		cmdparts = command.split(' ')
		cmd = None

		try:
			if cmdparts[0] in COMMANDS:
				cmd = COMMANDS[cmdparts[0]](*cmdparts[1:])

			if cmd is not None:
				self.commands.append(cmd)
				return cmd
			else:
				print 'Error Parsing Command "', command, '"'
		except Exception as e:
			print 'Critical error Parsing Command "', command, '" with the exception: \n', e.message

		return NOP()

	def dump_position(self):
		out = 'SSA ' +  str(self.current_scale) +  '\n'
		out += 'STR ' + str(self.offset.x) +  ' ' + str(self.offset.y) + '\n'
		for bone in self.bones():
			out += 'BRA ' + str(bone.name) + ' ' +  str(bone.theta) + '\n'
		return out

	def execute(self):
		for cmd in self.commands:
			if not cmd.tick(self):
				self.commands.remove(cmd)
		self.root.calculate_position()
		for orb in self.orbs():
			orb.calculate_position()

	def has_commands(self):
		if self.commands:
			return True
		else:
			return False

class Bone(object):
	def __init__(self, name, length, theta=0, parent=None, percent=1, theta_min = None, theta_max=None, scale=1.0):
		self.name = name
		self.skeleton = None

		self.length = length
		self.theta = theta

		self.parent = parent
		if self.parent is not None:
			self.parent.add_child(self)
		self.children = []

		self.percent = percent
		self.start_point = Point(0,0)
		self.scale = scale
		self.end_point = Point(self.get_length(),0)
		self.calculate_position()

		self.theta_min = theta_min
		self.theta_max = theta_max

	def get_length(self):
		return self.length * self.scale

	def get_all_children(self):
		all_children = [self]
		for child in self.children:
			all_children+= child.get_all_children()
		return all_children

	def rotate(self, alpha):
		#print 'rotating ', self.name, ' by ', alpha, ' degrees'
		new_theta = self.theta + alpha
		self.rotate_absolute(new_theta)

	def rotate_absolute(self, alpha):
		#print 'rotating ', self.name, ' by ', alpha, ' degrees'
		new_theta = alpha
		if self.theta_max is not None:
			if self.theta_min is not None:
				self.theta = min(max(self.theta_min, new_theta), self.theta_max)
			else:
				self.theta = min(new_theta, self.theta_max)
		else:
			if self.theta_min is not None:
				self.theta = max(self.theta_min, new_theta)
			else:
				self.theta = new_theta

	def effective_angle(self):
		dx = self.end_point.x - self.start_point.x
		dy = self.end_point.y - self.start_point.y
		if -SIGMA < dx < SIGMA:
			if dy > 0:
				return 90.0
			else:
				return 270.0
		else:
			if dx > 0:
				return math.atan(dy/dx)*180/math.pi
			else:
				return math.atan(dy/dx)*180/math.pi + 180

	def add_child(self, child):
		self.children.append(child)

	def get_connection_point(self, percent):
		dx = (self.end_point.x - self.start_point.x)*percent
		dy = (self.end_point.y - self.start_point.y)*percent
		return Point(self.start_point.x + dx,self.start_point.y + dy)

	def calculate_position(self):
		if self.parent is None:
			if self.skeleton is None:
				self.start_point = Point(0,0)
			else:
				self.start_point = self.skeleton.offset
			parent_angle = 0
			mirror_angle = 90
		else:
			self.start_point = self.parent.get_connection_point(self.percent)
			parent_angle = self.parent.effective_angle()
			mirror_angle = 0
		if self.skeleton is None:
			lr_mirror = False
		else:
			if self.skeleton.mirrorLR:
				lr_mirror = True
			else:
				lr_mirror = False
		if lr_mirror:
			ma = mirror_angle - (self.theta - mirror_angle)
			total_angle = ma + parent_angle
		else:
			total_angle = self.theta + parent_angle
		x = self.start_point.x +  self.get_length() * math.cos(math.pi * total_angle / 180)
		y = self.start_point.y + self.get_length() * math.sin(math.pi * total_angle / 180)

		self.end_point = Point(x, y)

		for child in self.children:
			child.calculate_position()

	def intersects_line(self, other):
		o1 = orientation(self.start_point, self.end_point, other.start_point)
		o2 = orientation(self.start_point, self.end_point, other.end_point)
		o3 = orientation(other.start_point, other.end_point, self.start_point)
		o4 = orientation(other.start_point, other.end_point, self.end_point)

		if (o1 != o2) and (o3 != o4):
			return True
		if o1 == 0 and on_segment(self.start_point, other.start_point, self.end_point):
			return True
		if o2 == 0 and on_segment(self.start_point, other.end_point, self.end_point):
			return True
		if o3 == 0 and on_segment(other.start_point, self.start_point, other.end_point):
			return True
		if o4 == 0 and on_segment(other.start_point, self.end_point, other.end_point):
			return True
		return False

	def intersects_point(self, point, tolerance):
		#if self.skeleton is not None:
			#point -= self.skeleton.offset
		px = self.end_point.x - self.start_point.x
		py = self.end_point.y - self.start_point.y

		something = px*px + py*py

		u =  ((point.x - self.start_point.x) * px + (point.y - self.start_point.y) * py) / float(something)
		if u > 1:
			u = 1
		elif u < 0:
			u = 0
		x = self.start_point.x + u * px
		y = self.start_point.y + u * py
		dx = x - point.x
		dy = y - point.y
		dist = math.sqrt(dx*dx + dy*dy)
		if dist < tolerance:
			return True
		else:
			return False

class Orb(object):
	theta = 0
	def __init__(self, name, radius, parent=None, percent=1, scale=1.0):
		self.name = name

		self.radius = radius

		self.parent = parent
		#if self.parent is not None:
			#self.parent.add_child(self)

		self.percent = percent
		self.start_point = Point(0,0)
		self.scale = scale
		self.end_point = Point(self.get_length(),0)
		self.calculate_position()

	def get_all_children(self):
		return [self]

	def get_length(self):
		return self.radius * self.scale

	def calculate_position(self):
		if self.parent is None:
			self.start_point = Point(0,0)
		else:
			self.start_point = self.parent.get_connection_point(self.percent)

	def intersects_line(self, other):
		l2 = other.get_length()**2
		t = (self.start_point.x - other.start_point.x) * (other.end_point.x - other.start_point.x)
		t += (self.start_point.y - other.start_point.y) * (other.end_point.y - other.start_point.y)
		t /= l2
		if t < 0:
			dist = self.start_point.distance(other.start_point)
		elif t > 1:
			dist = self.start_point.distance(other.end_point)
		else:
			nearpt = Point(other.start_point.x + t*(other.end_point.x - other.start_point.x), other.start_point.y + t*(other.end_point.y - other.start_point.y))
			dist = self.start_point.distance(nearpt)

		if dist <= self.get_length():
			return True
		else:
			return False

	def intersects_point(self, point):
		#if self.skeleton is not None:
			#point -= self.skeleton.offset
		px = self.end_point.x - self.start_point.x
		py = self.end_point.y - self.start_point.y

		something = px*px + py*py

		u =  ((point.x - self.start_point.x) * px + (point.y - self.start_point.y) * py) / float(something)
		if u > 1:
			u = 1
		elif u < 0:
			u = 0
		x = self.start_point.x + u * px
		y = self.start_point.y + u * py
		dx = x - point.x
		dy = y - point.y
		dist = math.sqrt(dx*dx + dy*dy)
		if dist < self.radius:
			return True
		else:
			return False