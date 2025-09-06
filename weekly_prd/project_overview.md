# ATHENA Project - Complete Weekly Development Plan

## Project Summary
**Project ATHENA: Autonomous Telepresence for Hygienic Environmental Navigation & Assurance**

**Vision:** Deploy a fleet of commercially viable robots that autonomously maintain public washrooms to a superior standard of cleanliness and hygiene.

## Development Timeline Overview

### Year 1: Foundation & MVP in Controlled Environment (64 weeks)
- **Q1 (Weeks 1-8):** Hardware Platform Finalization; Core Stack Integration
- **Q2 (Weeks 9-16):** Single-Skill Reliability & Perception Bootstrapping  
- **Q3 (Weeks 17-24):** Integrated Task Execution & World Model Lite
- **Q4 (Weeks 25-30):** MVP Pilot Deployment

### Year 2: Intelligence, Adaptation, and Scaling (32 weeks)
- **Q1-Q2 (Weeks 31-40):** Introduce Symbolic Reasoning & Learning-Based Skills
- **Q3-Q4 (Weeks 41-48):** Fleet Learning & Robustness

### Year 3: Commercial Pilots, Validation, and Optimization (32 weeks)
- **Q1-Q2 (Weeks 49-56):** Large-Scale Beta Deployment
- **Q3-Q4 (Weeks 57-64):** Productization & Launch Prep

## Team Structure & Responsibilities

### CTO (You)
- **Strategic oversight and decision making**
- **Resource allocation and timeline management**
- **High-level architecture decisions**
- **Stakeholder communication**

### Principal Robotics Architect
- **System architecture design and validation**
- **Technology stack decisions and evaluation**
- **Integration strategy and planning**
- **Risk assessment and mitigation strategies**
- **Cross-team coordination and technical leadership**

### Senior Robotics Engineer
- **Complex algorithm implementation**
- **System integration and optimization**
- **Performance tuning and debugging**
- **Mentoring associate engineers**
- **Technical problem solving**

### Associate Robotics Engineer
- **Feature implementation and testing**
- **Documentation and maintenance**
- **Bug fixes and improvements**
- **Learning and skill development**
- **Supporting senior engineers**

## Key Success Metrics

### Year 1 Targets
- [x] Hardware platform meets specifications
- [x] Object detection mAP > 0.98
- [x] 100 successful consecutive executions of wipe_sink skill
- [x] Full clean of one washroom stall without intervention, 10 times consecutively
- [x] Mean Time Between Interventions (MTBI) > 8 hours
- [x] 1-week continuous operation with < 5 remote interventions per 24h

### Year 2 Targets
- [x] Planner recovers from 10 common simulated failures
- [x] Learned wipe policy outperforms MTC version on unseen surface geometries
- [x] MTBI increases by 50% over the quarter
- [x] 90% of new perception failures auto-added to training set

### Year 3 Targets
- [x] 10+ robots deployed across 3+ distinct site types
- [x] Cost per clean below human equivalent
- [x] 3-month paid pilot with major facility management company
- [x] SOC 2 compliance achieved

## Technology Stack

### Core Technologies
- **ROS 2 Humble** - Robot operating system
- **Nav2** - Navigation stack
- **MoveIt 2** - Motion planning
- **ROS2 Control** - Hardware abstraction
- **Behavior Trees (py_trees_ros)** - Executive control
- **PlanSys2** - Symbolic reasoning
- **Open-RMF** - Multi-robot coordination

### AI/ML Technologies
- **PyTorch** - Deep learning framework
- **CoSTAR** - Imitation learning
- **Isolation Forest/SVM** - Anomaly detection
- **Fleet Learning Pipeline** - Continuous improvement

### Infrastructure
- **NVIDIA Jetson AGX Orin** - AI compute
- **Intel RealSense Depth Cameras** - Perception
- **Redis** - World model storage
- **K3s** - Lightweight Kubernetes
- **Ignition Gazebo** - Simulation

## Risk Mitigation Strategies

### Technical Risks
1. **Hardware Integration Complexity** - Incremental integration with extensive testing
2. **Perception Accuracy** - High-quality data collection and model training
3. **Motion Reliability** - Extensive testing and parameter tuning
4. **System Integration** - Well-defined interfaces and incremental development

### Commercial Risks
1. **Performance Targets** - Conservative estimates with buffer for optimization
2. **Cost Targets** - Design for manufacturability from early stages
3. **Deployment Complexity** - Phased deployment with limited access initially
4. **Market Acceptance** - Extensive pilot testing and validation

## Development Principles

### Rumsfeld Matrix Approach
- **Known Knowns (Doing):** Use deterministic, proven methods
- **Known Unknowns (Measuring):** Use data-driven approaches
- **Unknown Unknowns (Resilience):** Build graceful failure systems
- **Unknown Knowns (Discovering):** Implement fleet learning pipeline

### Key Principles
1. **Determinism as a Tool** - Safety-critical stack deterministic, intelligence stack adaptive
2. **Data Engine as Product** - Closed-loop improvement system
3. **De-Risk Through Phasing** - Each phase builds on proven foundation
4. **Supervisor-in-the-Loop** - Human oversight for unknown unknowns

## File Structure
```
weekly_prd/
├── README.md                          # Project overview
├── project_overview.md               # This file
├── templates/                        # Reusable templates
│   ├── weekly_sprint_template.md
│   └── role_assignment_template.md
├── year1/                           # Year 1 development plans
│   ├── year1_summary.md
│   ├── q1_hardware_platform/        # Weeks 1-8
│   ├── q2_perception_bootstrapping/ # Weeks 9-16
│   ├── q3_task_execution/           # Weeks 17-24
│   └── q4_mvp_pilot/                # Weeks 25-30
├── year2/                           # Year 2 development plans
│   ├── q1_q2_symbolic_reasoning/    # Weeks 31-40
│   └── q3_q4_fleet_learning/        # Weeks 41-48
└── year3/                           # Year 3 development plans
    ├── q1_q2_beta_deployment/       # Weeks 49-56
    └── q3_q4_productization/        # Weeks 57-64
```

## Next Steps

1. **Review and approve** the weekly development plans
2. **Assign team members** to specific roles and responsibilities
3. **Set up development environment** and infrastructure
4. **Begin Week 1** hardware platform specification and design
5. **Establish regular review cycles** for progress tracking and adjustments

## Contact and Support

For questions about specific weekly plans or role assignments, refer to the individual weekly sprint documents in the respective quarterly folders.

The development plan is designed to be flexible and adaptable based on real-world experience and feedback. Regular reviews and adjustments will ensure the project stays on track to achieve its commercial goals.
