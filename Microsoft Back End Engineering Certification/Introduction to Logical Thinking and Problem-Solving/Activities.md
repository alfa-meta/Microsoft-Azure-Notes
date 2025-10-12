
## Logic Visualisation

1. Problem: Need to validate if number is even or odd
2. Key processes: 
	a. Prompt user to input a number
	b. Make sure the input is an integer number
	c. Output whether its even or odd
3. 

4. Pseudocode:
	1. Start the process to check if a number is even or odd
	2. Prompt the user to input number
	3. Make sure the input is a number if not:
		a. Return to instruction 2.
	4. Check if the number is divisible by 2:
		a. If the number is divisible by 2 print "Even number"
		b. if the number is not divisible by 2 print "Odd number"
	5. End process

## Using Deductive Reasoning to Write Pseudocode

Example Problem Statement:
You are tasked with creating a program to check if a given year is a leap year.
A year is a leap year if its divisible by 4, but not every year divisible by 100 is a leap year unless its also divisible by 400.

Example Problem Statement:
Imagine you are creating a program to determine the letter grade for a student based on their numerical score.


Problem 1: Integers
Write pseudocode to create a program that determines whether a number is positive, negative, or zero.

Start
	Ask user for number
	Make sure the input given is a number
	If number is > 0
		Return to user positive
		End
	If number is < 0
		Return to user negative
		End
	Otherwise
		Return to user zero
		End
End


Problem 2: Senior Discount
Write pseudocode to create a program that checks whether a person is eligible for a senior citizen discount.

Start
	Ask user for age
	Make sure its an integer number
	If users age is 65 or higher
		Display to user "Eligible for senior discount"
	Otherwise
		Display to user "Not eligible for senior discount"
End


## Problem Decomposition Using Top-Down Approach and Modularisation

Problem 1 Statement:

Decompose the development of a fitness tracking app that can track users workouts, monitor their diet, and provide health insights.

Instructions:
1. Define the main goal of the application.
2. Use the top-down approach to break down the app into its major features
3. Further decompose each feature into smaller, manageable tasks.
4. Identify opportunities for modularisation.


Solution
1. Main goal is to Develop a fitness tracking app.

2. Split into 4 main parts:
	1. Visualisation of the app
	2. Tracking Workouts
	3. Monitoring Diet
	4. Providing health insights
3. Decompose each feature into manageable tasks:
	1. Create a dashboard for workouts, diet, and insights
	2. Allow users to submit their workouts
	3. Allow users to submit their food
	4. Allow users to view AI feedback relating to health.
4. Modularisation:
	1. Dashboard is 1 view that shows 3 different components defined below
	2. Workouts are submitted via a form, showing length, Name of exercise(s) and days it occurred.
	3. Food is submitted via a form showing, name, time of day/week, calories.
	4. Visual assistant suggesting improvements and showing things that are going well.

## Problem Decomposition Using Top-Down and Bottom-Up Approaches


Problem 1: Project Management

1. Main goal is to create a Project Management tool that allows for task tracking, team collaborations, reporting.
2. Top-down
3. Features and Systems:
	1. Task tracking
	2. Team chat
	3. Team call
	4. Report of Team and task activities
	5. Top down was chosen because the end goal was known, so all the tasks added were in relation to achieving that goal.

Problem 2: Online Health Monitoring System

1. Main goal is to create an online health monitoring system that tracks physical activity, sleep, and heart rate.
2. Bottom up
3. Implement:
	1. Tracking of Physical activity
		1. Track steps taken a day
		2. Track all exercises
		3. Track weight
	2. Track Sleep
		1. Track sleep duration
		2. Track sleep cycles
		3. Track when person wakes up and goes to sleep
	3. Track heart rate
4. Bottom up is more appropriate because the individual features are more important and the absolute scope of the application is not known


## Activity: Pseudocode

### Problem 1

Start
	Input Maths marks
	Input Science marks
	Input English marks
	Add all marks together called `TotalMarks`
	`TotalMarks` should be divided by the number of subjects
	Print `TotalMarks` / number of subjects
End

Start
	Take user input as `[name]`
	Print(`[name]`)
End
