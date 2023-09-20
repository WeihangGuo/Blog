+++ 
draft = true
date = 2022-04-24T03:35:15+08:00
title = "F1tenth: Automatic Emergency Braking"
description = "The goal of this blog is to develop a safety node for the race cars that will stop the car from collision when travelling at higher velocities. We will implement Time to Collision using the LaserScan message in the simulator."
slug = ""
authors = []
tags = []
categories = []
externalLink = ""
series = []
katex  = true
+++

{{<bilibili bv1SR4y1N7Lf>}}

![Automatic Emergency Braking](https://raw.githubusercontent.com/baboonSTW/Blog-img/main/202204240350512.png)

```python
#!/usr/bin/env python
# Author: Weihang Guo
# Date: 2/28/2022

import rospy
import numpy as np
from sensor_msgs.msg import LaserScan
from nav_msgs.msg import Odometry
from std_msgs.msg import Bool
from ackermann_msgs.msg import AckermannDriveStamped, AckermannDrive
# TODO: import ROS msg types and libraries

class Safety(object):
    """
    The class that handles emergency braking.
    """
    def __init__(self):
        """
        One publisher should publish to the /brake topic with a AckermannDriveStamped brake message.
        One publisher should publish to the /brake_bool topic with a Bool message.
        You should also subscribe to the /scan topic to get the LaserScan messages and
        the /odom topic to get the current speed of the vehicle.
        The subscribers should use the provided odom_callback and scan_callback as callback methods
        NOTE that the x component of the linear velocity in odom is the speed
        """
        self.THRESHOLD = 0.4 
        self.ranges = []
        self.angles = []
        self.angles_cos = []
        self.speed = 0
        self.angle_min = 0
        self.angle_max = 0
        self.angle_increment = 0
        
        self.initialized = False

        rospy.Subscriber('scan', LaserScan, self.scan_callback)
        rospy.Subscriber('odom', Odometry, self.odom_callback)

    def odom_callback(self, odom_msg):
        # TODO: update current speed
        self.speed = odom_msg.twist.twist.linear.x

    def scan_callback(self, scan_msg):
        # TODO: calculate TTC
        # TODO: publish brake message and publish controller bool
        self.ranges =np.array(scan_msg.ranges)
        # rospy.loginfo(np.min(self.ranges))
        if self.initialized == False:
        # read and save angle_min, angle_max, angle_increment
            self.angle_min = scan_msg.angle_min
            self.angle_max = scan_msg.angle_max
            self.angle_increment = scan_msg.angle_increment
            self.angles = np.linspace(self.angle_min,self.angle_max,len(self.ranges))
            # rospy.loginfo(len(self.ranges))
            self.angles_cos = np.cos(self.angles)
            # self.angles_cos = np.where(self.angles_cos>0, self.angles_cos, 0)
            self.initialized = True
        # rospy.loginfo(self.angles_cos)
        self.calculate_TTC() 
    def calculate_TTC(self):
        if self.speed < 0.1 and self.speed > -0.1:
            return 
        subspeeds = self.angles_cos*self.speed
        # TTCs = np.divide(self.ranges, subspeeds,out=np.zeros_like(self.ranges), where=subspeeds!=0)
        TTCs = np.divide(self.ranges, subspeeds)
        # TTC = np.min(TTCs[np.nonzero(TTCs)])
        lower_than_threhold = False
        for TTC in TTCs:
            if TTC > 0 and TTC < self.THRESHOLD:
                lower_than_threhold = True
                break
        # rospy.loginfo(TTC)
        if lower_than_threhold:
            pub_brake_bool = rospy.Publisher("brake_bool", Bool, queue_size=10)
            msg_brake_bool = Bool(data=True)
            pub_brake_bool.publish(msg_brake_bool)

            pub_brake = rospy.Publisher("brake", AckermannDriveStamped, queue_size=10)
            drive = AckermannDrive(speed=0)
            msg_brake = AckermannDriveStamped(drive=drive)
            pub_brake.publish(msg_brake)
            self.speed = 0
            # rospy.loginfo("warning")
    def calculate_TTC_1(self):
        pass
def main():
    rospy.init_node('safety_node')
    sn = Safety()
    rospy.spin()
if __name__ == '__main__':
    main()
```

