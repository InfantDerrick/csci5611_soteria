# Project 3 CSCI 5611

- Infant Derrick Gnana Susairaj (gnana014)

### About

For part 2:

I decided I wanted to make one of those noodle arm things that are in front of car shows and it would be colliding with a wheel. Pretty simple using Inverse Kinematics.

### Code

You can access the code [here](https://github.com/InfantDerrick/csci5611/tree/master/projects/proj3). 

#### Inverse Kinematics
```processing
    public void inverseKinematics(Vec2 dest) {
    Vec2 toDest, toEnd;
    float theta;
    for(int i = 0; i < 4; i++){
      toDest = dest.minus(start.get(i));
      if (i == 0 && toDest.length() < 0.0001) return;
      toEnd = end.minus(start.get(i));
      theta = acos(clamp(dot(toDest.normalized(), toEnd.normalized()), -1, 1)) * speedScales.get(i);
      if (cross(toDest, toEnd) < 0) 
        jointAngles.set(i, jointAngles.get(i) + theta);
      else 
        jointAngles.set(i, jointAngles.get(i) - theta);
      if (lowerLimits.get(i) == 0 && upperLimits.get(i) == 0){}
      else{
        if (jointAngles.get(i) < lowerLimits.get(i))
          jointAngles.set(i, lowerLimits.get(i));
        else if (jointAngles.get(i) > upperLimits.get(i))
          jointAngles.set(i, upperLimits.get(i));
      }
      forwardKinematics();
    }
  }
 }
```
#### Forward Kinematics
```processing
      public void forwardKinematics() {
    start.set(1, new Vec2(cos(jointAngles.get(0)) * linkLengths.get(0), sin(jointAngles.get(0)) * linkLengths.get(0)).plus(start.get(0)));
    start.set(2, new Vec2(cos(jointAngles.get(0) + jointAngles.get(1)) * linkLengths.get(1), sin(jointAngles.get(0) + jointAngles.get(1)) * linkLengths.get(1)).plus(start.get(1)));
    start.set(3, new Vec2(cos(jointAngles.get(0) + jointAngles.get(1) + jointAngles.get(2)) * linkLengths.get(2), sin(jointAngles.get(0) + jointAngles.get(1) + jointAngles.get(2)) * linkLengths.get(2)).plus(start.get(2)));
    end = new Vec2(cos(jointAngles.get(0) + jointAngles.get(1) + jointAngles.get(2) + jointAngles.get(3)) * linkLengths.get(3), sin(jointAngles.get(0) + jointAngles.get(1) + jointAngles.get(2) + jointAngles.get(3)) * linkLengths.get(3)).plus(start.get(3));
  }
```

### Media

[Main Demo (Dance)](https://youtu.be/DqYLLQlvAAQ).

[Collision Demo](https://youtu.be/G7Fb5YU53-E).
# Project 3 CSCI 5611

- Infant Derrick Gnana Susairaj (gnana014)

### About

For part 2:

I decided I wanted to make one of those noodle arm things that are in front of car shows and it would be colliding with a wheel. Pretty simple using Inverse Kinematics.

### Code

You can access the code [here](https://github.com/InfantDerrick/csci5611/tree/master/projects/proj3). 

#### Inverse Kinematics
```processing
    public void inverseKinematics(Vec2 dest) {
    Vec2 toDest, toEnd;
    float theta;
    for(int i = 0; i < 4; i++){
      toDest = dest.minus(start.get(i));
      if (i == 0 && toDest.length() < 0.0001) return;
      toEnd = end.minus(start.get(i));
      theta = acos(clamp(dot(toDest.normalized(), toEnd.normalized()), -1, 1)) * speedScales.get(i);
      if (cross(toDest, toEnd) < 0) 
        jointAngles.set(i, jointAngles.get(i) + theta);
      else 
        jointAngles.set(i, jointAngles.get(i) - theta);
      if (lowerLimits.get(i) == 0 && upperLimits.get(i) == 0){}
      else{
        if (jointAngles.get(i) < lowerLimits.get(i))
          jointAngles.set(i, lowerLimits.get(i));
        else if (jointAngles.get(i) > upperLimits.get(i))
          jointAngles.set(i, upperLimits.get(i));
      }
      forwardKinematics();
    }
  }
 }
```
#### Forward Kinematics
```processing
      public void forwardKinematics() {
    start.set(1, new Vec2(cos(jointAngles.get(0)) * linkLengths.get(0), sin(jointAngles.get(0)) * linkLengths.get(0)).plus(start.get(0)));
    start.set(2, new Vec2(cos(jointAngles.get(0) + jointAngles.get(1)) * linkLengths.get(1), sin(jointAngles.get(0) + jointAngles.get(1)) * linkLengths.get(1)).plus(start.get(1)));
    start.set(3, new Vec2(cos(jointAngles.get(0) + jointAngles.get(1) + jointAngles.get(2)) * linkLengths.get(2), sin(jointAngles.get(0) + jointAngles.get(1) + jointAngles.get(2)) * linkLengths.get(2)).plus(start.get(2)));
    end = new Vec2(cos(jointAngles.get(0) + jointAngles.get(1) + jointAngles.get(2) + jointAngles.get(3)) * linkLengths.get(3), sin(jointAngles.get(0) + jointAngles.get(1) + jointAngles.get(2) + jointAngles.get(3)) * linkLengths.get(3)).plus(start.get(3));
  }
```

### Media

[Main Demo (Dance)](https://youtu.be/DqYLLQlvAAQ).

[Collision Demo](https://youtu.be/G7Fb5YU53-E).
