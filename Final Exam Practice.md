1. **Question:** Explain the steps and pseudocode of different functions for the following embedded system case study.
	- "Automated Wheel Chair"
	- **Answer:**
		- **Initialization:** Set up sensors, actuators, and communication interfaces.
		```pseudocode
		function initialize_system():
			setup_motor_controllers()
			calibrate_sensors()
			init_display()
			display_message("System Ready")
			return
		```
		- **User Input:** Capture and process input from joystick or other interfaces.
		```pseudocode
		function get_user_input():
			input_data = read_joystick() # Example: Get X and Y coordinates
			if input_data == "UP":
				return "MOVE_FORWARD"
			elif input_data == "DOWN":
				return "MOVE_BACKWARD"
			elif input_data == "LEFT":
				return "TURN_LEFT"
			elif input_data == "RIGHT":
				return "TURN_RIGHT"
			elif input_data == "STOP":
				return "STOP"
			else:
				return "NO_COMMAND"
		```
		- **Obstacle Detection:** Use sensors to detect obstacles and avoid collisions.
		```pseudocode
		function check_obstacles():
			distance = read_ultrasonic_sensor()
			if distance < CRITICAL_DISTANCE:
				return True # Obstacle detected
			else:
				return False
		```
		- **Motor Control:** Control motors to execute movement commands.
		```pseudocode
		function control_motors(command):
			if command == "MOVE_FORWARD":
				set_motor_speed(LEFT_MOTOR, SPEED)
				set_motor_speed(RIGHT_MOTOR, SPEED)
			elif command == "MOVE_BACKWARD":
				set_motor_speed(LEFT_MOTOR, -SPEED)
				set_motor_speed(RIGHT_MOTOR, -SPEED)
			elif command == "TURN_LEFT":
				set_motor_speed(LEFT_MOTOR, -TURN_SPEED)
				set_motor_speed(RIGHT_MOTOR, TURN_SPEED)
			elif command == "TURN_RIGHT":
				set_motor_speed(LEFT_MOTOR, TURN_SPEED)
				set_motor_speed(RIGHT_MOTOR, -TURN_SPEED)
			elif command == "STOP":
				stop_motors()
			return
		```
		- **Emergency Stop:** Immediately stop the wheelchair in critical situations.
		```pseudocode
		function emergency_stop():
			stop_motors()
			display_message("Emergency Stop Activated")
			alert_user()
			return
		```
		- **Main Control Loop:** Integrate all functions and control the wheelchair in real time.
		```pseudocode
		function main_loop():
			initialize_system()
			
			while True:
				command = get_user_input()
				if command == "STOP":
					emergency_stop()
					break
				
				if check_obstacles():
					control_motors("STOP")
					display_message("Obstacle Detected")
				else:
					control_motors(command)
		```
2. **Question:** Write an interrupt-driven program (pseudocode) to perform the following activities.
	- "Earliest Deadline First (EDF) Scheduling" and "Rate Monotonic (RM) Scheduling"
	- **Answer:**
		- **Interrupt-Driven EDF Scheduler:**
		```pseudocode
		# Timer Interrupt Service Routine (ISR)
		function timer_ISR():
			current_time += 1 # Increment system time
			update_deadlines() # Adjust deadlines for EDF
			
			# Check for task deadlines and activate tasks
			for task in tasks:
				if current_time % task.period == 0: # Periodic task activation
					task.remaining_time = task.execution_time
					task.deadline = current_time + task.period # Update absolute deadline
					
			run_EDF_scheduler()
			return
			
		# EDF Scheduler Function
		function run_EDF_scheduler():
			# Select the task with the earliest deadline
			active_task = None
			earliest_deadline = MAX_VALUE
			for task in tasks:
				if task.remaining_time > 0 and task.deadline < earliest_deadline:
					active_task = task
					earliest_deadline = task.deadline
					
			# Execute the selected task
			if active_task:
				execute_task(active_task)
			return
			
		# Execute Task
		function execute_task(task):
			if task.remaining_time > 0:
				task.remaining_time -= 1
				if task.remaining_time == 0:
					log("Task", task.id, "completed at", current_time)
			return
		```
		- **Interrupt-Driven RM Scheduler:**
		```pseudocode
		# Timer Interrupt Service Routine (ISR)
		function timer_ISR():
			current_time += 1 # Increment system time
			
			# Activate periodic tasks based on their period
			for task in tasks:
				if current_time % task.period == 0: # Periodic task activation
					task.remaining_time = task.execution_time
					
			run_RM_scheduler()
			return
			
		# RM Scheduler Function
		function run_RM_scheduler():
			# Select the highest-priority task with remaining time
			active_task = None
			highest_priority = MAX_VALUE
			for task in tasks:
				if task.remaining_time > 0 and task.priority < highest_priority:
					active_task = task
					highest_priority = task.priority
					
			# Execute the selected task
			if active_task:
				execute_task(active_task)
			return
			
		# Execute Task
		function execute_task(task):
			if task.remaining_time > 0:
				task.remaining_time -= 1
				if task.remaining_time == 0:
					log("Task", task.id, "completed at", current_time)
			return
		```
3. **Question:** Three performance evaluation techniques are present: analytic modelling, simulation, and measurement. How will these techniques vary between the design of the following two embedded systems?
	- "Automated wheel chair" vs "smart kitchen systems"
	- **Answer:** 
		- **Automated Wheelchair vs Smart Kitchen System:**

| Criterion            | Analytic modelling | Simulation | Measurement           |
| -------------------- | ------------------ | ---------- | --------------------- |
| Stage                | Any                | Varies     | Post-prototype        |
| Time required        | Varies             | Varies     | Large                 |
| Tools                | Analysis           | C          | Varies                |
| Accuracy             | Medium             | High       | Very high             |
| Trade-off evaluation | Easy               | Medium     | Time consuming (hard) |
| Cost                 | Low                | Medium     | High                  |
| Saleability          | Low                | Medium     | High                  |

| Criterion            | Analytic modelling | Simulation | Measurement           |
| -------------------- | ------------------ | ---------- | --------------------- |
| Stage                | Any                | Varies     | Post-prototype        |
| Time required        | Low                | Varies     | Large                 |
| Tools                | Analysis           | Python     | Varies                |
| Accuracy             | Medium             | High       | Very high             |
| Trade-off evaluation | Medium             | High       | Time consuming (hard) |
| Cost                 | Low                | Medium     | High                  |
| Saleability          | Low                | Medium     | High                  |
4. **Question:** Assume that you have to certify an automated speed camera machine according to IEC 61508. Perform hazard and risk analysis, and then evaluate the safety integrity level.
	- **Answer:**
		- **Safety Integrity Level (SIL) Evaluation:** IEC 61508 defines SIL levels (1-4) based on the required reliability of safety functions. Defining risk parameters for consequences (3, severe), frequency and exposure (2, frequent), probability of avoidance (2, low), and probability of occurrence (2, probable), the combination of these parameters results in SIL 2 for critical functions like speed detection and data integrity.
		- **Hazard and Risk Analysis (and Hazard Mitigation):**

| Hazard                              | Cause                            | Impact                                  | Likelihood |
| ----------------------------------- | -------------------------------- | --------------------------------------- | ---------- |
| Failure to detect speeding vehicles | Radar malfunction or obstruction | Speeding vehicles go undetected         | Moderate   |
| False speeding detection            | Radar calibration errors         | Incorrect penalties issued              | Moderate   |
| Data loss                           | Power failure or storage error   | Missing evidence for violations         | Moderate   |
| Communication failure               | Network or hardware issue        | Data not transmitted to law enforcement | Low        |
| Unauthorized access                 | Weak security protocols          | System misuse, data breaches            | Moderate   |

| Hazard                              | Severity | Likelihood | Risk Level | Comments                                  |
| ----------------------------------- | -------- | ---------- | ---------- | ----------------------------------------- |
| Failure to detect speeding vehicles | High     | Moderate   | Medium     | Affects enforcement effectiveness.        |
| False speeding detection            | High     | Moderate   | Medium     | Legal implications for wrong penalties.   |
| Data loss                           | Moderate | Moderate   | Medium     | Potential loss of critical evidence.      |
| Communication failure               | Low      | Low        | Low        | Data can be temporarily stored locally.   |
| Unauthorized access                 | High     | Moderate   | Medium     | Risk of tampering and privacy violations. |

| Hazard                              | Mitigation Measure                                                                  |
| ----------------------------------- | ----------------------------------------------------------------------------------- |
| Failure to detect speeding vehicles | Implement radar redundancy and self-diagnostics.                                    |
| False speeding detection            | Calibrate radar periodically and implement cross-validation using multiple sensors. |
| Data loss                           | Use uninterruptible power supply (UPS) and redundant storage.                       |
| Communication failure               | Implement retry mechanisms and local storage buffers for temporary data retention.  |
| Unauthorized access                 | Use encryption, multi-factor authentication, and intrusion detection systems.       |
5. **Question:** Between hardware and software monitors, which one will you use for debugging or performance evaluation of the embedded code of the automated wheel chair? Explain the differences in terms of different parameters that can influence the decision.
	- **Answer:** 
		- **Hardware Monitors:** Are used for low-level performance analysis, timing analysis, and debugging hardware interfaces.
		- **Software Monitors:** Are used for high-level code debugging, performance profiling, and cost-sensitive scenarios.
		- **Recommended Approach:** For the automated wheelchair, I believe a hardware monitor-based approach is ideal for evaluating the system. We may analyse low-level performance metrics like motor response time, sensor latency, and interrupt timings, as well as verify real-time deadlines to ensure safety-critical operations (e.g., stopping when an obstacle is detected). This provides a comprehensive evaluation, helping to ensure system robustness and safety.