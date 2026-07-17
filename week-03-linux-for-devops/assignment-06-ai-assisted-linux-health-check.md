# Assignment 6 — Build an AI-Assisted Linux Health Check (AI-Assisted Linux Incident Triage)

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will build a read-only Bash triage script that checks the health of your Ubuntu server and Nginx application, connect it to Claude Code as a reusable `/linux-triage` skill, simulate a controlled Nginx incident, use the skill to gather and analyze evidence, recover the service manually, and verify recovery. The workflow follows the Agentic Loop: Gather → Analyze → Human Act → Verify.

---

# Task 1 — Confirm the Healthy Baseline and Create the Workspace

## Goal

Confirm that Nginx and the React application are healthy before building the automation.

### Evidence

#### Screenshot 1 — Output of `systemctl is-active nginx`, `ss -ltn | grep ':80'`, and `curl -I http://localhost`

Add your screenshot here.

![Screenshot1](<Screenshot 1_assignment6_task1.png>)

---

#### Screenshot 2 — Output of `pwd` and `find . -maxdepth 4 -type d | sort` showing the workspace folder structure

Add your screenshot here.

![Screenshot2](<Screenshot 2_assignment6_task1.png>)

---

### Notes

Answer the following in your own words:

**1. What proves that Nginx is running?**

Add your answer here.

Running the command systemctl is-active nginx returns the literal word active. This explicit status from Ubuntu's system manager confirms that the background Nginx daemon is successfully running.

---

**2. What proves that the server is listening for HTTP traffic?**

Add your answer here.

The command ss -ltn | grep ':80' outputs a row showing 0.0.0.0:80 (or \*:80) in a LISTEN state. Additionally, executing curl -I http://localhost successfully streams back an HTTP/1.1 200 OK header response directly from the web server.

---

**3. Why must you capture a healthy baseline before simulating an incident?**

Add your answer here.

Capturing a healthy baseline is critical because it establishes a clear reference point for normal behavior. Without documented metrics of a functioning environment (such as expected open ports, baseline RAM use, and active system service states), it is incredibly difficult to pinpoint abnormalities or trace root causes accurately when an outage occurs.

---

# Task 2 — Create Project Context and Safety Rules in CLAUDE.md

## Goal

Tell Claude exactly what this project does and what it is not allowed to do.

### Evidence

#### Screenshot 3 — CLAUDE.md open in VS Code showing all four sections (Project Overview, Incident Workflow, Safety Rules, Output Rules)

Add your screenshot here.

![Screenshot3](<Screenshot 3_assignment6_task2.png>)

---

### Notes

Answer the following in your own words:

**1. Why should Claude receive project-specific operational rules?**

Add your answer here.

Claude requires explicit operational rules to prevent it from hallucinating actions, making assumptions, or performing destructive operations on a live server. Clear boundaries ensure the AI functions strictly as a predictable, safe analytical assistant tailored to this specific triage task.

---

**2. Why is the human required to execute the recovery command?**

Add your answer here.

A human operator is required to execute recovery actions to maintain safety and accountability. AI models can easily misinterpret a complex state or apply a fix that unintentionally worsens an outage; keeping a human in the loop ensures that any system modification is validated by an engineer before execution.

---

**3. Which rule prevents Claude from making an unsupported diagnosis?**

Add your answer here.

Section 4, Output Rules, explicitly states that root-cause conclusions must be strictly coupled with concrete log outputs or metric values, and forbids speculative assertions without underlying evidentiary data.

---

# Task 3 — Use Agentic AI to Plan Before Writing the Script

## Goal

Use Claude Code to inspect the environment and produce a read-only plan before creating any Bash code.

### Evidence

#### Screenshot 4 — Claude Code showing the five-check plan and read-only inspection results

Add your screenshot here.
![Screenshot4](<Screenshot 4_assignment6_task3.png>)
![Screenshot4b](<Screenshot 4b_assignment6_task3.png>)

---

### Notes

Answer the following in your own words:

**1. Which part of this task represents the Gather phase?**

Add your answer here.

The Gather phase is represented by Claude actively reading our local server state, checking which services are running, and assessing the structure of the directories to understand what telemetry points are available for analysis.

---

**2. Did Claude follow the instruction not to create files? How did you verify this?**

Add your answer here.

Yes, Claude followed the instruction perfectly. I verified this by running ls -la after exiting the session, which confirmed that no new files or prototype scripts had been written to my workspace directory during the planning phase.

---

**3. Why is planning before coding useful in DevOps automation?**

Add your answer here.
Planning before coding forces you to define clear parameters, thresholds, and predictable logic paths upfront. In production environments, this eliminates coding edge-case mistakes, keeps scripts modular, and ensures that your automation collects the exact telemetry points required to diagnose an issue without introducing system overhead or unintended behavior.

---

# Task 4 — Build the Linux Triage Bash Script

## Goal

Create one Bash script that gathers consistent Linux and Nginx health evidence.

### Evidence

#### Screenshot 5 — Top section of `linux-triage.sh` showing variables, thresholds, and the checks array

Add your screenshot here.

![Screenshot5](<Screenshot 5_assignment6_task4.png>)

---

#### Screenshot 6 — Middle section showing check functions and conditionals

Add your screenshot here.

![Screenshot6](<Screenshot 6_assignment6_task4.png>)

---

#### Screenshot 7 — Bottom section showing the loop, summary function, and exit behavior

Add your screenshot here.

![Screenshot7](<Screenshot 7_assignment6_task4.png>)

---

7

#### Screenshot 8 — Output of `bash -n scripts/linux-triage.sh` (no syntax errors) and `ls -l scripts/linux-triage.sh` showing executable permission

Add your screenshot here.

![Screenshot8](<Screenshot 8_assignment6_task4.png>)

---

### Notes

Answer the following in your own words:

**1. What is stored in the checks array?**

Add your answer here.

The checks array stores string literals matching the exact names of our five custom shell functions (check_cpu_mem, check_disk, check_nginx_service, check_port_80, and check_http_availability).

---

**2. How does the `for` loop use that array?**

Add your answer here.

The for loop dynamically cycles through each string entry inside the array one by one, sets it as an executable instruction pointer, and triggers that matching bash function to run its telemetry collectors sequentially.

---

**3. Why are the health checks separated into functions?**

Add your answer here.

Separating the checks into modular functions isolates the logic blocks. This makes the automation script cleanly readable, allows you to debug individual metric tests independently, and makes expanding the triage script with additional system checks easy in the future.

---

**4. What is the purpose of `$(...)` in this script?**

Add your answer here.

The $(...) syntax represents command substitution. It tells the shell execution engine to run the command inside the parentheses first and pass the text output directly back into a local variable or echo line.

---

**5. Why does the script use different exit codes for HEALTHY, WARN, and FAIL?**

Add your answer here.

Using unique exit codes allows external orchestration workflows (like Claude Code, GitHub Actions, or monitoring pipelines) to instantly evaluate the severity of an incident programmatically without scraping human-readable text logs.

---

# Task 5 — Run and Understand the Healthy-State Report

## Goal

Run the Bash script against the healthy server and verify that it creates a report.

### Evidence

#### Screenshot 9 — Output of `./scripts/linux-triage.sh` showing your Full Name and all five check results

Add your screenshot here.

![Screenshot9](<Screenshot 9_assignment6_task5.png>)

---

#### Screenshot 10 — Output showing the captured exit code and final summary

Add your screenshot here.

![Screenshot10](<Screenshot 10_assignment6_task5.png>)

---

### Notes

Answer the following in your own words:

**1. What is the overall status of your healthy baseline?**

Add your answer here.

The overall system status is Healthy. All resource metrics sit safely below warning thresholds, system services are running, and ports are correctly bound.

---

**2. Which exact Linux evidence proves the application is serving traffic?**

Add your answer here.

The report explicitly captures an active status of active from the Nginx system manager, verifies that the network layer has socket port 80 in a listening state, and streams back an authoritative 200 HTTP status loopback connection check.

---

**3. Did your script return exit code 0 or 1? Explain why.**

Add your answer here.

The script returned exit code 0. This code explicitly signals across POSIX/Linux standard conventions that a program successfully executed to completion without encountering any structural warning or critical error states.

---

**4. What is the difference between a warning and a failure in this script?**

Add your answer here.

A warning indicates that a resource check (like CPU, memory, or disk usage) has exceeded an initial baseline caution threshold, prompting attention, but the application is still up and serving traffic. A failure means a service is completely inactive, port 80 is not bound, or a critical metric has crossed its maximum operational ceiling, causing an actual service outage.

---

# Task 6 — Create and Run the /linux-triage Skill

## Goal

Turn the Bash script into a reusable, manually invoked Agentic AI workflow.

### Evidence

#### Screenshot 11 — `SKILL.md` showing the frontmatter, allowed tool restrictions, and safety rules

Add your screenshot here.

![Screenshot11](<Screenshot 11_assignment6_task6.png>)

---

#### Screenshot 12 — `/linux-triage` output for the healthy server

Add your screenshot here.

![Screenshot12](<Screenshot 12_assignment6_task6.png>)

---

### Notes

Answer the following in your own words:

**1. Why does this skill have Bash, Read, and Grep, but not Write?**

Add your answer here.

The skill is strictly limited to bash, read, and grep to enforce a zero-mutation safety policy. By deliberately omitting the write capability, we make it mechanically impossible for the AI to accidentally alter production configurations, delete system log streams, or deploy untested hotfixes automatically.

---

**2. Why is `disable-model-invocation: true` useful for this skill?**

Add your answer here.

Setting disable-model-invocation: true locks down the tool execution pipeline. It instructs the engine to run the designated local triage script directly rather than wasting tokens or risking unexpected behavior by asking the LLM to creatively guess how to perform the health scan.

---

**3. What part is performed by Bash, and what part is performed by Claude?**

Add your answer here.

Bash executes the hard work of gathering low-level infrastructure data (scraping network sockets, measuring memory targets, checking service state tables). Claude functions as the analytical layer—reading the output, matching trends against our runbook rules, and explaining what the raw metrics mean in plain, professional language.

---

**4. Why is this better than asking Claude "Is my server healthy?" without giving it evidence?**

Add your answer here.

Asking an AI about server health without providing real context leads to generic advice or hallucinations since the model cannot see your infrastructure. Using a skill binds the model's logic to precise local facts, guaranteeing that its conclusions match the actual runtime state of your server.

---

# Task 7 — Simulate an Nginx Incident and Let the Skill Diagnose It

## Goal

Create a controlled service failure, gather evidence through Bash, and let Claude analyze the evidence without taking recovery action.

### Evidence

#### Screenshot 13 — Output showing Nginx is inactive and the HTTP request fails

Add your screenshot here.

![Screenshot13](<Screenshot 13_assignement6_task7.png>)

---

#### Screenshot 14 — `/linux-triage` output showing failed evidence, most likely cause, and a suggested recovery command

Add your screenshot here.

![Screenshot14](<Screenshot 14_assignment6_task7.png>)
![Screenshot14b](<Screenshot 14b_assignment6_task7.png>)

---

#### Screenshot 15 — `incident-failure-report.txt` showing the failed checks and your Full Name

Add your screenshot here.

![Screenshot15](<Screenshot 15_assignment6_task7.png>)

---

### Notes

Answer the following in your own words:

**1. Which three checks failed?**

Add your answer here.

1. Which three checks failed?
   The three checks that failed are:

check_nginx_service (Nginx status returned inactive)

check_port_80 (Port 80 was not actively bound or listening)

check_http_availability (The curl endpoint check failed to receive an HTTP 200 OK code)

---

**2. What evidence supports the conclusion that Nginx is unavailable?**

Add your answer here.

The system daemon explicitly reported a status of inactive, network state tracing confirmed that no process was listening on Port 80, and loopback HTTP traffic dropped completely, returning a connection refused error rather than a 200 web header.

---

**3. Did Claude execute the recovery command? Why is that important?**

Add your answer here.

No, Claude did not execute the recovery command. This behavior is incredibly critical because it confirms that the system's safety rules (CLAUDE.md) and tool limits are functioning properly—acting as a secure, read-only observer rather than making unverified, automated modifications to a production server environment.

---

**4. Which phase of the Agentic Loop is represented by the Bash report?**

Add your answer here.

The Bash report represents the Gather phase, where local shell commands systematically collect cold, hard system metrics and store them as baseline context.

---

**5. Which phase is represented by Claude's explanation?**

Add your answer here.

Claude's explanation represents the Analyze phase, where the model parses the raw text logs, evaluates data against predefined thresholds, correlates the failure conditions, and determines the most probable root cause.

---

# Task 8 — Recover Manually, Verify Again, and Write the Incident Summary

## Goal

Recover the service as the human operator and prove that the system is healthy again.

### Evidence

#### Screenshot 16 — Output showing Nginx is active and `curl -I http://localhost` returns 200 OK

Add your screenshot here.

![Screenshot16](<Screenshot 16_assignment6_task8.png>)

---

#### Screenshot 17 — Second `/linux-triage` output showing successful recovery with no FAIL results

Add your screenshot here.

![Screenshot17](<Screenshot 17_assignment6_task8.png>)

---

#### Screenshot 18 — Output of `ls -lah reports` showing both `incident-failure-report.txt` and `recovery-report.txt`

Add your screenshot here.

![Screenshot18](<Screenshot 18_assignment6_task8.png>)

---

#### Screenshot 19 — `incident-summary.md` showing all required sections and your Full Name

Add your screenshot here.

![Screenshot19](<Screenshot 19_assignment6_task8.png>)

---

### Notes

Answer the following in your own words:

**1. What action did you execute manually?**

Add your answer here.

I manually executed sudo systemctl start nginx to restart the web server daemon.

---

**2. What evidence proves that the service recovered?**

Add your answer here.

The service status returned to active, port 80 resumed listening for incoming traffic, and local curl checks successfully returned a valid HTTP/1.1 200 OK web header response.

---

**3. Why is the second triage run necessary?**

Add your answer here.

The second run is critical to close the Verify phase of our loop. It ensures that restarting the service fixed the main issue without creating secondary system bottlenecks, such as high memory allocation or CPU spikes.

---

**4. What could go wrong if an AI agent automatically restarted every failed service?**

Add your answer here.

If an AI blindly restarts services automatically, it can mask severe underlying bugs or trigger destructive crash loops. For example, if a service is failing because of a corrupted database or a full disk, continuously restarting it could permanently corrupt data, overwrite debugging logs, or cause an extended, unstable outage.

---

**5. In one sentence, explain the difference between using AI as a chatbot and using AI in this agentic workflow.**

Add your answer here.

Using AI as a chatbot simply generates text answers based on static training data, whereas using AI in an agentic workflow connects it to active terminal tools to gather real-time data, reason through local facts, and assist in live system troubleshooting under strict safety boundaries.

---

# Incident Summary

Fill in all seven sections below in your own words.

**Full Name:** David Agada Adikwu

**Date:** 17/07/2026

---

**1. Reported Symptom**

Add your answer here.

The web application went offline, resulting in service unavailability. External HTTP requests targeting the application server began failing completely.

---

**2. Evidence Collected**

Add your answer here.

- `systemctl is-active nginx` returned `inactive`.
- `ss -ltn | grep ':80'` showed no active listener bound to port 80.
- `curl -I http://localhost` returned a connection refused failure instead of an HTTP 200 OK header.
- The `linux-triage.sh` automation script exited with a Critical Failure code (Exit Code 2).

---

**3. Most Likely Cause**

Add your answer here.

The background Nginx service daemon was manually stopped or terminated, which unbinded port 80 and brought down the application layer.

---

**4. Human-Approved Recovery Action**

Add your answer here.

A human operator manually intervened and ran `sudo systemctl start nginx` to bring the service daemon back online.

---

**5. Verification**

Add your answer here.

- `systemctl is-active nginx` confirmed a status of `active`.
- `curl -I http://localhost` returned a successful `HTTP/1.1 200 OK` header response.
- A secondary execution of the `/linux-triage` automation suite passed all 5 system checks cleanly, exiting with a completely healthy status (Exit Code 0).

---

**6. Safety Decision**

Add your answer here.

The system safety rules defined in `CLAUDE.md` and tool blocks inside `SKILL.md` correctly restricted Claude Code to a read-only role.The AI identified the issue and provided the correct fix but waited for human confirmation and execution, preventing unauthorized or automated modifications to the infrastructure.

---

**7. Agentic Loop Mapping**

Add your answer here.

- **GATHER:** The local Bash triage script collected service states, port mappings, and endpoint telemetry.
- **ANALYZE:** Claude Code parsed the telemetry blocks, pinpointed the inactive Nginx daemon, and proposed the exact recovery command.
- **HUMAN ACT:** The human engineer verified the analysis and safely executed the recovery command manually.
- **VERIFY:** The triage script was re-run post-recovery to confirm that all infrastructure checks returned to a healthy baseline.

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

# GitHub Repository URL

Paste the URL of your GitHub folder or repository containing the assignment files here:

`_______________`
Blog link

## https://open.substack.com/pub/david227779/p/reflection-week-3-just-showing-up?r=8qepx8&utm_campaign=post&utm_medium=web&showWelcomeOnShare=true

# Submission Instructions

- Add all required screenshots in your submission
- Full Name must be visible in required screenshots and the Bash report
- All written answers must be in your own words
- Do not expose sensitive information (keys, passwords, AWS account IDs, tokens)
- GitHub URL must be included in this document

---

# Completion Checklist

- [ ] Task 1: Healthy baseline confirmed, workspace created (Screenshots 1–2, Notes answered)
- [ ] Task 2: CLAUDE.md created with all four sections (Screenshot 3, Notes answered)
- [ ] Task 3: Five-check plan produced by Claude using read-only tools (Screenshot 4, Notes answered)
- [ ] Task 4: `linux-triage.sh` created, syntax validated, executable permission set (Screenshots 5–8, Notes answered)
- [ ] Task 5: Healthy-state report generated with no FAIL result (Screenshots 9–10, Notes answered)
- [ ] Task 6: `/linux-triage` skill created and run successfully on healthy server (Screenshots 11–12, Notes answered)
- [ ] Task 7: Nginx incident simulated, failed evidence captured, Claude did not execute recovery (Screenshots 13–15, Notes answered)
- [ ] Task 8: Nginx recovered manually, recovery verified, reports saved, incident summary complete (Screenshots 16–19, Notes answered)
- [ ] Incident summary contains all seven required sections
- [ ] LinkedIn post published and URL submitted
- [ ] Full Name visible in all required screenshots and the Bash report
- [ ] Skill does not have Write permission
- [ ] Skill did not execute any recovery commands
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
