# PRD: PlanSys2 Integration & Symbolic Reasoning
**Principal Robotics Architect - Product Requirements Document**

**Document Version:** 2.0  
**Date:** July 30, 2024  
**Phase:** Year 2, Q1-Q2 - Introduce Symbolic Reasoning & Learning-Based Skills  
**Sprint:** Weeks 31-32 (July 30 - August 12, 2024)  
**Deliverable:** Screen recording due in 48 hours

---

## Executive Summary

### Project Objective
Implement PlanSys2 symbolic reasoning on a TurtleBot3 with ROS2 Humble, Nav2, and Behavior Trees to demonstrate intelligent navigation and cleaning simulation in a washroom environment.

### Success Criteria
- TurtleBot3 setup with ROS2 Humble + Nav2 + Behavior Trees
- PlanSys2 integration with PDDL domain for washroom cleaning
- Robot navigates to basin → performs cleaning motion → navigates to commode → performs cleaning motion
- Recovery behavior demonstration when navigation fails
- **Screen recording delivered within 48 hours**

---

## Technical Requirements

### 1. Hardware Setup
**Robot Platform:** TurtleBot3 Burger/Waffle  
**Sensors:** 
- LiDAR (RPLIDAR A1)
- Intel RealSense D435i camera
- IMU and wheel encoders

### 2. Software Stack
**Base System:**
- Ubuntu 22.04 LTS
- ROS2 Humble
- Nav2 navigation stack
- Behavior Trees (py_trees_ros)
- PlanSys2 symbolic planner

**World Model:** Provided URDF and world files  
**Environment:** Simple washroom simulation with basin and commode

### 3. PDDL Domain Specification

#### 3.1 Domain Structure
```pddl
(define (domain athena-washroom-cleaning)
  (:requirements :strips :typing :equality)
  
  (:types
    robot location basin commode
  )
  
  (:predicates
    (at ?robot ?location)
    (near_basin ?robot)
    (near_commode ?robot)
    (basin_clean ?basin)
    (commode_clean ?commode)
    (cleaning_complete)
  )
  
  (:action navigate_to_basin
    :parameters (?robot ?from ?to)
    :precondition (and (at ?robot ?from) (not (near_basin ?robot)))
    :effect (and (at ?robot ?to) (near_basin ?robot))
  )
  
  (:action clean_basin
    :parameters (?robot ?basin)
    :precondition (near_basin ?robot)
    :effect (and (basin_clean ?basin) (not (near_basin ?robot)))
  )
  
  (:action navigate_to_commode
    :parameters (?robot ?from ?to)
    :precondition (and (at ?robot ?from) (not (near_commode ?robot)))
    :effect (and (at ?robot ?to) (near_commode ?robot))
  )
  
  (:action clean_commode
    :parameters (?robot ?commode)
    :precondition (near_commode ?robot)
    :effect (and (commode_clean ?commode) (not (near_commode ?robot)) (cleaning_complete))
  )
)
```

#### 3.2 Problem Definition
```pddl
(define (problem washroom-cleaning-problem)
  (:domain athena-washroom-cleaning)
  
  (:objects
    robot1 - robot
    start_pos - location
    basin_area - location
    commode_area - location
    basin1 - basin
    commode1 - commode
  )
  
  (:init
    (at robot1 start_pos)
    (not (near_basin robot1))
    (not (near_commode robot1))
    (not (basin_clean basin1))
    (not (commode_clean commode1))
    (not (cleaning_complete))
  )
  
  (:goal
    (and
      (basin_clean basin1)
      (commode_clean commode1)
      (cleaning_complete)
    )
  )
)
```

### 4. Behavior Tree Integration

#### 4.1 BT Structure
```
Root
├── Sequence
│   ├── NavigateToBasin
│   ├── CleanBasinMotion
│   ├── NavigateToCommode
│   └── CleanCommodeMotion
└── Fallback
    ├── PlanSys2Recovery
    └── EmergencyStop
```

#### 4.2 Recovery Behavior
```python
class PlanSys2Recovery(BehaviorTree):
    def __init__(self):
        self.plansys2_client = PlanSys2Client()
        
    def execute(self):
        if self.navigation_failed():
            # Generate recovery plan using PlanSys2
            recovery_plan = self.plansys2_client.generate_plan()
            return self.execute_recovery_plan(recovery_plan)
        return SUCCESS
```

---

## Implementation Plan

### Phase 1: Environment Setup (4 hours)
**Objective:** Set up TurtleBot3 with ROS2 Humble and required packages

**Tasks:**
1. **TurtleBot3 Setup**
   - Install ROS2 Humble on Ubuntu 22.04
   - Install TurtleBot3 packages and dependencies
   - Configure LiDAR and RealSense camera
   - Test basic robot functionality

2. **Navigation Stack Setup**
   - Install and configure Nav2
   - Set up SLAM and localization
   - Configure navigation parameters
   - Test navigation in provided world

**Deliverables:**
- TurtleBot3 running with ROS2 Humble
- Nav2 navigation working
- LiDAR and camera data streaming
- Basic navigation in washroom world

### Phase 2: Behavior Tree Integration (4 hours)
**Objective:** Implement behavior trees for cleaning task execution

**Tasks:**
1. **Behavior Tree Setup**
   - Install py_trees_ros
   - Create cleaning behavior tree
   - Implement navigation and cleaning actions
   - Test behavior tree execution

2. **Action Implementation**
   - NavigateToBasin action
   - CleanBasinMotion action (2-3 second circular motion)
   - NavigateToCommode action
   - CleanCommodeMotion action (2-3 second circular motion)

**Deliverables:**
- Behavior tree implementation
- Cleaning motion actions
- Basic task execution working

### Phase 3: PlanSys2 Integration (6 hours)
**Objective:** Integrate PlanSys2 for symbolic reasoning and recovery

**Tasks:**
1. **PlanSys2 Setup**
   - Install PlanSys2 packages
   - Configure PDDL domain and problem
   - Set up PlanSys2 ROS2 interface
   - Test plan generation

2. **Recovery Behavior**
   - Implement PlanSys2 recovery mechanism
   - Create fallback behavior tree
   - Test recovery scenarios
   - Integrate with main behavior tree

**Deliverables:**
- PlanSys2 integrated with ROS2
- PDDL domain and problem working
- Recovery behavior implemented
- Full system integration

### Phase 4: Testing & Recording (2 hours)
**Objective:** Test complete system and create demonstration video

**Tasks:**
1. **System Testing**
   - Test complete cleaning sequence
   - Test recovery behavior
   - Verify all requirements met
   - Performance optimization

2. **Video Recording**
   - Record complete demonstration
   - Show navigation to basin
   - Show cleaning motion at basin
   - Show navigation to commode
   - Show cleaning motion at commode
   - Show recovery behavior if applicable

**Deliverables:**
- Complete system working
- Screen recording of demonstration
- Documentation of implementation

---

## Technical Specifications

### 1. Robot Configuration
```yaml
# turtlebot3.yaml
turtlebot3:
  model: burger
  sensors:
    lidar: rplidar_a1
    camera: realsense_d435i
    imu: true
  navigation:
    planner: nav2_planner
    controller: nav2_controller
    recovery: nav2_recovery
```

### 2. PDDL Action Implementation
```python
class CleanBasinMotion(Action):
    def __init__(self):
        super().__init__("CleanBasinMotion")
        self.cmd_vel_pub = self.create_publisher(Twist, '/cmd_vel', 10)
        
    def execute(self):
        # Perform 2-3 second circular cleaning motion
        start_time = time.time()
        while (time.time() - start_time) < 3.0:
            twist = Twist()
            twist.linear.x = 0.1
            twist.angular.z = 0.5
            self.cmd_vel_pub.publish(twist)
            time.sleep(0.1)
        
        # Stop robot
        twist = Twist()
        self.cmd_vel_pub.publish(twist)
        return SUCCESS
```

### 3. Recovery Behavior
```python
class PlanSys2Recovery(BehaviorTree):
    def __init__(self):
        self.plansys2_client = PlanSys2Client()
        self.recovery_actions = [
            "retry_navigation",
            "replan_path",
            "emergency_stop"
        ]
    
    def execute(self):
        if self.navigation_failed():
            recovery_plan = self.plansys2_client.generate_recovery_plan()
            return self.execute_recovery_plan(recovery_plan)
        return SUCCESS
```

---

## Success Criteria

### Functional Requirements
- [ ] TurtleBot3 navigates to basin area
- [ ] Robot performs 2-3 second cleaning motion at basin
- [ ] Robot navigates to commode area
- [ ] Robot performs 2-3 second cleaning motion at commode
- [ ] Recovery behavior works when navigation fails
- [ ] PlanSys2 generates valid plans for cleaning sequence

### Technical Requirements
- [ ] ROS2 Humble + Nav2 + Behavior Trees running
- [ ] LiDAR and RealSense camera data streaming
- [ ] PDDL domain and problem files working
- [ ] PlanSys2 integration functional
- [ ] Screen recording completed within 48 hours

### Quality Requirements
- [ ] Code is well-documented and modular
- [ ] Error handling implemented for all actions
- [ ] System is robust and handles failures gracefully
- [ ] Video clearly demonstrates all required functionality

---

## Risk Mitigation

### High Risk: Integration Complexity
**Mitigation:** Start with simple examples, use provided world model, test incrementally

### Medium Risk: Time Constraints
**Mitigation:** Focus on core functionality, use existing examples, parallel development

### Low Risk: Hardware Issues
**Mitigation:** Use simulation first, have backup hardware ready

---

## Deliverables

1. **Working System:** Complete TurtleBot3 setup with PlanSys2 integration
2. **PDDL Files:** Domain and problem definition files
3. **Behavior Tree:** Complete behavior tree implementation
4. **Recovery Behavior:** PlanSys2 recovery mechanism
5. **Screen Recording:** 5-10 minute demonstration video
6. **Documentation:** Implementation notes and code comments

---

## Timeline

**Total Time:** 16 hours over 48 hours  
**Phase 1:** 4 hours (Environment Setup)  
**Phase 2:** 4 hours (Behavior Tree Integration)  
**Phase 3:** 6 hours (PlanSys2 Integration)  
**Phase 4:** 2 hours (Testing & Recording)

**Deadline:** Screen recording due in 48 hours from project start

---

## Conclusion

This PRD provides a focused, practical approach to implementing PlanSys2 symbolic reasoning on a TurtleBot3 platform. The 48-hour timeline requires efficient execution and focus on core functionality. The success of this implementation will demonstrate the feasibility of symbolic reasoning for cleaning robots and provide a foundation for more complex behaviors in future phases.

The key to success will be leveraging existing examples and focusing on the core demonstration requirements while maintaining code quality and documentation standards.
