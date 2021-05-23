#----------------------------------------------------------
#
# Solution is approach to solve task#5 of Computer Vision challenge and is presented here.
#
# Solution realises ability to Preview of the road based on LIDAR data and can detect height
# differences of the road ahead.
#
# Algorithm gets a LIDAR's points cloud from the road and use those to compute height differences.
# It is taken into account that data are presented of 4 values: x, y, z, i
# +x is to the left of the car, +y is to the back of the car, +z is to the ground (downwards).
#
# For data acquisition we chose rectangle area ahead the vehicle and closestly as near
# as possible to the vehicle. We did so because we tried to collect data with higher accuracy
# relate to hieght-deviation of the road surface.
#
# Algorithm was created for to be embeded to manual_control.py python script of The CLARA
# simulation app.
#
#---------------------------------------------------------

#********************************
#  added code for task#5 Preview of the road
#********************************

LIDAR_MOUNT_HEIGHT = 2000
TRESHOLD_HEIGHT_DIF = 80

# we collect Z-value of lidar-points only for those one that are in rectangle zone ahead the vehicle

point_counter, sq_sum = 0, 0
for x,y,z,_ in points:  # note that +x is to the left of the car, +y is to the back of the car, +z is to the ground (downwards).
    if ((-1500<x<+1500) and (-3000>y>-4500)):
        # we prepare data to evaluate RMS for  lidar-points' heights
        sq_sum = (abs(z)-LIDAR_MOUNT_HEIGHT)**2
        point_counter += 1

#we evaluate RMS for  lidar-points' heights
sq_deviation = abs(sq_sum/(point_counter))
mean_deviation = (sq_deviation**0.5)*1.6 # for probability 0.95 according to Laplace distribution
print(mean_deviation)

# alarm for car driver
if (mean_deviation >= TRESHOLD_HEIGHT_DIF ):
    print("/\/\/\/\/\!!rugged road!!/\/\/\/\/\n")

#********************************