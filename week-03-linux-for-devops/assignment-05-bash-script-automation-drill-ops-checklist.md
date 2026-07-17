# Assignment 5 — Bash Script Automation Drill (OPS Checklist)

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will practice Bash scripting by building a series of small automation scripts covering environment setup, variables, arrays, loops, file conditionals, if-else logic, and functions. These scripts form the foundation of real-world Linux automation used in DevOps, cloud, and production support environments.

---

# Task 1 — Bash Environment & Workspace Setup

## Goal

Verify that Bash is available on your system and create a clean workspace for this assignment.

### Evidence

#### Screenshot 1 — Output of `echo $SHELL` and `bash --version`

Add your screenshot here.

![Screenshot1](<Screenshot 1_assignmnet5_task1.png>)

---

#### Screenshot 2 — Output of `pwd` and `ls -lah` showing the scripts directory

Add your screenshot here.

![Screenshot2](<Screenshot 2_assignment5_task1.png>)

---

### Notes

Answer the following in your own words:

**1. What is Bash?**

Add your answer here.

Bash stands for Bourne Again Shell. It is a command-line text interface and interpreter that reads the commands you type in your terminal and tells your Linux operating system exactly what to do. It also acts as a full programming language used to automate repetitive server tasks.

---

**2. What is the difference between shell and Bash?**

Add your answer here.

A shell is a broad term for any command-line program that lets a user talk to the operating system's kernel. Bash is just one specific, highly popular type of shell. For instance "shell" as the general category of vehicles, and "Bash" as a specific model of car.

---

**3. Why is it important to confirm the Bash version before writing scripts?**

Add your answer here.

It is critical because newer versions of Bash introduce cleaner syntax, shortcuts, and safety features that old versions don't support. Checking the version upfront ensures that the scripts you write won't unexpectedly crash or fail when deployed onto different server environments.

---

# Task 2 — Your First Bash Script

## Goal

Create your first Bash script, make it executable, and run it from the terminal.

### Evidence

#### Screenshot 1 — Content of `first-script.sh`

Add your screenshot here.

![Screenshot1](<Screenshot 1_assignment5_task2.png>)

---

#### Screenshot 2 — Output of `./first-script.sh`

Add your screenshot here.

![Screenshot2](<Screenshot 2_assignment5_task2-1.png>)

---

#### Screenshot 3 — Output of `ls -l first-script.sh` showing executable permission

Add your screenshot here.

![Screenshot3](<Screenshot 3_assignment5_task2.png>)

---

### Notes

Answer the following in your own words:

**1. What is the purpose of `#!/bin/bash`?**

Add your answer here.

Pronounced as "She-bang". It tells the Linux operating system exactly which interpreter to use when reading the file. By pointing to /bin/bash, it ensures the server uses Bash to execute our code instead of any other system shell.

---

**2. Why do we use `chmod +x` before running a script?**

Add your answer here.

By default, Linux creates new text files with read and write permissions only, preventing them from being run as programs for security reasons. Running chmod +x explicitly modifies the file's permissions, adding the executable flag so the system allows it to run as a functional script.

---

**3. What is the difference between running a script using `./script.sh` and `bash script.sh`?**

Add your answer here.

Running ./script.sh tells the system to treat the file as a standalone program, relying entirely on the internal shebang (#!/bin/bash) line to figure out how to run it. Running bash script.sh directly forces the system to run the file through Bash, ignoring whatever shebang or execute permissions are on the file.

---

# Task 3 — Variables: User Information Script

## Goal

Use variables to store and display user-related information.

### Evidence

#### Screenshot 1 — Content of `user-info.sh`

Add your screenshot here.

![Screenshot1](<Screenshot 1_assignment5_task3.png>)

---

#### Screenshot 2 — Output of `./user-info.sh`

Add your screenshot here.

![Screenshot2](<Screenshot 2_assignment5_task3.png>)

---

### Notes

Answer the following in your own words:

**1. What is a variable in Bash?**

Add your answer here.

A variable is a temporary storage label or container used by a script to hold numbers, words, or file paths. Instead of typing the same text repeatedly, you store it in a variable once so you can easily reference or update it later in your script.

---

**2. Why should we avoid spaces around the `=` sign when creating variables?**

Add your answer here.

Bash uses spaces to separate distinct commands from their arguments. If you add spaces around the equals sign (like NAME = David), Bash gets confused and assumes that NAME is an independent command you are trying to run, which causes a syntax error.

---

**3. How do you access the value stored inside a Bash variable?**

Add your answer here.

You access the value of a variable by placing a dollar sign ($) right before its name. For example, typing echo DEVELOPER_NAME just prints the word itself, but typing echo $DEVELOPER_NAME extracts and prints the actual value stored inside that variable.

---

# Task 4 — Arrays & Loops: Tools Checklist Script

## Goal

Use arrays and loops to print a checklist of tools used in Bash scripting.

### Evidence

#### Screenshot 1 — Content of `tools-checklist.sh`

Add your screenshot here.

![Screenshot1](<Screenshot 1_assignent5_task4.png>)

---

#### Screenshot 2 — Output of `./tools-checklist.sh`

Add your screenshot here.

![Screenshot2](<Screenshot 2_assignment5_task4.png>)

---

### Notes

Answer the following in your own words:

**1. What is an array in Bash?**

Add your answer here.

An array is an ordered list variable that lets you store multiple distinct values or data strings inside a single container. Instead of creating separate individual variables for every single item, you can group them together under one name.

---

**2. Why are arrays useful in scripts?**

Add your answer here.

Arrays are incredibly useful because they allow you to write dynamic scripts that can manage collections of items effortlessly.

---

**3. What does `"${tools[@]}"` mean?**

Add your answer here.

In Bash, the syntax [@] tells the system to grab every single item stored inside the array at once, while the double quotes ensure that items containing spaces are treated correctly as individual elements.

---

**4. What is the purpose of the `for` loop in this script?**

Add your answer here.

The for loop acts as an automated cycle. It steps through the array one element at a time, copies the current item into a temporary variable (tool), runs the echo command on it, and automatically moves to the next item until it hits the end of the list.

---

# Task 5 — Loops: Number Counter Script

## Goal

Use loops to repeat a task multiple times.

### Evidence

#### Screenshot 1 — Content of `counter.sh`

Add your screenshot here.

![Screenshot1](<Screenshot 1_assignment5_task5.png>)

---

#### Screenshot 2 — Output of `./counter.sh`

Add your screenshot here.

![Screenshot2](<Screenshot 2_assignment5_task5.png>)

---

### Notes

Answer the following in your own words:

**1. What is a loop?**

Add your answer here.

A loop is a programming instruction that tells your system to repeat a specific block of code multiple times. It will continue running the task over and over until a set limit or condition is reached.

---

**2. Why do we use loops in Bash scripting?**

Add your answer here.

We use loops in Bash to eliminate manual repetition. If you need to spin up 5 virtual servers, delete 20 old log files, or check 50 website URLs, a loop lets you write the execution command exactly once and let the server cycle through the work instantly.

---

**3. How many times did the loop run in your script?**

Add your answer here.

The loop executed exactly 5 times, counting systematically from 1 through 5 as defined by the bracket range.

---

**4. What would you change if you wanted the loop to run 10 times?**

Add your answer here.

I would modify the range inside the braces from {1..5} to {1..10}. This tells the loop expression to cycle ten times instead of five.

---

# Task 6 — Files & Conditionals: File Validation Script

## Goal

Use file checks and conditionals to verify whether files and directories exist.

### Evidence

#### Screenshot 1 — Output of `ls -lah ../test-folder`

Add your screenshot here.

![Screenshot1](<Screenshot 1_assignment5_task6.png>)

---

#### Screenshot 2 — Content of `file-check.sh`

Add your screenshot here.

![Screenshot2](<Screenshot 2_assignment5_task6.png>)

---

#### Screenshot 3 — Output of `./file-check.sh`

Add your screenshot here.

![Screenshot3](<Screenshot 3_assignment5_task6.png>)

---

### Notes

Answer the following in your own words:

**1. What does `-d` check in Bash?**

Add your answer here.

The -d flag is a file test operator that checks if a specified path exists and is specifically a directory (a folder) rather than a regular file.

---

**2. What does `-f` check in Bash?**

Add your answer here.

The -f flag is a file test operator that checks if a specified path exists and is a regular file (like a script, text file, or image) rather than a folder or a shortcut link.

---

**3. Why should file and directory paths be stored in variables?**

Add your answer here.

Storing paths in variables makes your scripts clean and easily reusable. If a folder location changes down the line, you only have to update the path once at the top of your script instead of digging through lines of code to change it everywhere.

---

**4. What happens if the file does not exist?**

Add your answer here.

If the file does not exist, the conditional check fails, skipping the first block of code and jumping straight to the else block, which prints out our custom [X] ERROR message.

---

# Task 7 — Conditionals: Pass or Retry Script

## Goal

Use if-else conditionals to make decisions based on a variable value.

### Evidence

#### Screenshot 1 — Content of `score-check.sh` with `score=85`

Add your screenshot here.

![Screenshot1](<Screenshot 1_assignment5_task7.png>)

---

#### Screenshot 2 — Output showing `Result: Pass`

Add your screenshot here.

![Screenshot2](<Screenshot 2_assignment5_task7.png>)

---

#### Screenshot 3 — Content of `score-check.sh` with `score=55`

Add your screenshot here.

![Screenshot3](<Screenshot 3_assignment5_task7.png>)

---

#### Screenshot 4 — Output showing `Result: Retry`

Add your screenshot here.

![Screenshot4](<Screenshot 4_assignmnet5_task7.png>)

---

### Notes

Answer the following in your own words:

**1. What is the purpose of if-else in Bash?**

Add your answer here.

The purpose of an if-else statement is to introduce decision-making capabilities into your scripts. It tells the system: "If this specific condition is true, execute the first block of code; otherwise, execute the alternative block."

---

**2. What does `-ge` mean?**

Add your answer here.

The -ge flag stands for Greater than or Equal to. It is an integer comparison operator used in Bash to compare numerical values.

---

**3. Why should conditions be tested with different values?**

Add your answer here.

Testing conditions with different inputs guarantees that your script's logic is sound and robust. It confirms that the script handles both positive and negative outcomes correctly, preventing false positives where broken code accidentally passes.

---

**4. How can conditionals help in automation scripts?**

Add your answer here.

Conditionals are the backbone of smart automation. They allow a script to handle real-world variations dynamically—such as checking if a server's disk space is over 90% before clearing logs, verifying if a backup service succeeded before archiving files, or determining whether to send an alert if a service goes down.

---

# Task 8 — Functions: Final Bash Automation Script

## Goal

Create a final Bash script using functions to organize reusable code.

### Evidence

#### Screenshot 1 — Content of `final-automation.sh`

Add your screenshot here.

![Screenshot1](<Screenshot 1_assignment5_task8.png>)

---

#### Screenshot 2 — Output of `./final-automation.sh`

Add your screenshot here.

![Screenshot2](<Screenshot 2_assignment5_task8.png>)

---

#### Screenshot 3 — Output of `ls -lah` showing all created scripts

Add your screenshot here.

![Screenshot3](<Screenshot 3_assignment5_task8.png>)

---

### Notes

Answer the following in your own words:

**1. What is a function in Bash?**

Add your answer here.

A function is a self-contained block of code designed to perform a specific task. You define it once at the top of your script under a clear name, and you can call or execute that entire block anywhere later in the script simply by typing its name.

---

**2. Why are functions useful in scripts?**

Add your answer here.

Functions stop you from rewriting the same blocks of code over and over. They keep your automation scripts organized, modular, and easy to read. If you need to change how an environment check works, you only have to update it inside the function rather than editing ten different lines throughout the script.

---

**3. Which functions did you create in this script?**

Add your answer here.

I created two distinct functions:

1.  check_environment: Validates the existence of project folders and runtime log files.

2.  verify_tools: Iterates through a defined list of components to confirm stack readiness.

---

**4. How does this final script combine variables, arrays, loops, conditionals, files, and functions?**

Add your answer here.

This master script brings all core programming foundations together seamlessly:

1.  Functions wrap our tasks into modular blocks (check_environment and verify_tools).

2.  Variables hold our target folder and file strings ($WORKSPACE and $LOG_FILE).

3.  Conditionals and File checks use an if-else block joined with -d and -f flags to confirm files are present.

4.  Arrays hold our list of platform components (REQUIRED_TOOLS).

5.  Loops use a for structure to systematically iterate through that array and print out each verified tool.

---

# LinkedIn Post (Required)

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`__________________________`

---

#### Screenshot — Published LinkedIn post

Add your screenshot here.

---

# Submission Instructions

- Add all required screenshots in your submission
- Full name must be visible in required screenshots
- All script files must be created and run successfully
- Required notes must be answered clearly for every task
- Do not expose sensitive information (keys, passwords, credentials)

---

# Completion Checklist

- [ ] Task 1: Environment setup verified, workspace created (Screenshots 1–2, Notes answered)
- [ ] Task 2: First script created, executed, permissions verified (Screenshots 1–3, Notes answered)
- [ ] Task 3: Variables script created and run (Screenshots 1–2, Notes answered)
- [ ] Task 4: Arrays and loops script created and run (Screenshots 1–2, Notes answered)
- [ ] Task 5: Counter loop script created and run (Screenshots 1–2, Notes answered)
- [ ] Task 6: File validation script created and run (Screenshots 1–3, Notes answered)
- [ ] Task 7: Pass/Retry conditional script tested with both values (Screenshots 1–4, Notes answered)
- [ ] Task 8: Final automation script created and run (Screenshots 1–3, Notes answered)
- [ ] All scripts run without errors
- [ ] Full Name visible in all required screenshots
- [ ] LinkedIn post published and URL submitted
- [ ] No sensitive data exposed

---

## 📌 About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory) focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.

---

## 📌 Resources

- 🌐 DMI Official Website: https://pravinmishra.com/dmi
- 🎓 DevOps for Beginners (Udemy): https://www.udemy.com/course/devops-for-beginners-docker-k8s-cloud-cicd-4-projects/
- 🎓 Agentic AI DevOps with Claude Code: https://www.udemy.com/course/ultimate-agentic-ai-devops-with-claude-code/
- 🎓 DevOps with Claude Code: Terraform, EKS, ArgoCD & Helm: https://www.udemy.com/course/devops-with-claude-code-terraform-eks-argocd-helm/
- ▶️ YouTube Playlist: https://www.youtube.com/playlist?list=PLFeSNDtI4Cho
- 🔗 Pravin Mishra (LinkedIn): https://www.linkedin.com/in/pravin-mishra-aws-trainer/
- 🏢 CloudAdvisory (LinkedIn): https://www.linkedin.com/company/thecloudadvisory/

---

_This submission is part of DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track._
