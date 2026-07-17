# Assignment 3 — Production Maintenance Drill (OPS Checklist)

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will treat your already deployed React application (on Ubuntu VM with Nginx) as a live production system. You will perform structured operational checks covering network validation, service health, log analysis, resource monitoring, configuration verification, and incident simulation with recovery — mirroring real on-call DevOps responsibilities.

---

# Task 1 — Server Access & Networking Validation

## Goal

Verify that the deployed React application is reachable from the browser and confirm basic network connectivity of the Ubuntu VM.

### Evidence

#### Screenshot 1 — Browser showing the React app with your Full Name visible on the UI

Add your screenshot here.
![Screenshot1](<Screenshot 1_assignment3_task1.png>)

---

#### Screenshot 2 — Output of `ip a`

Add your screenshot here.
![Screenshot2](<Screenshot 2_assignment3_task1.png>)

---

#### Screenshot 3 — Output of `sudo ss -tulpen`

Add your screenshot here.
![Screenshot3](<Screenshot 3_assignment3_task1.png>)

---

#### Screenshot 4 — Output of `sudo ufw status`

Add your screenshot here.
![Screenshot4](<Screenshot 4_assignment3_task1.png>)

---

### Notes

Answer the following in your own words:

**1. What proves Nginx is listening on 0.0.0.0:80?**

Write your answer here.

In the output of the sudo ss -tulpen command, there is a line showing 0.0.0.0:80 (or \*:80) with the state set to LISTEN. The program associated with this line is listed as nginx. This proves that the web server is actively waiting for web traffic on port 80.

---

**2. What proves SSH is active on port 22?**

Write your answer here.

Looking at the same network connections list (ss), there is a line showing 0.0.0.0:22 (or \*:22) in a LISTEN state, handled by sshd. This confirms that the SSH daemon is running and ready to accept remote secure logins.

---

**3. Did you find any unexpected open ports? Explain briefly.**

Write your answer here.

No, I didn't find any unexpected ports. The only open ports were port 22 for SSH (which we need to access the server) and port 80 for Nginx (which we need to serve our website). This is ideal because leaving unused ports open creates security risks.

---

# Task 2 — Service Health & Systemd Validation (Nginx)

## Goal

Verify that Nginx is properly installed, running, enabled at boot, and safely configured.

### Evidence

#### Screenshot 1 — Output of `systemctl status nginx --no-pager`

Add your screenshot here.
![Screenshot1](<Screenshot 1_assignment3_task2.png>)

---

#### Screenshot 2 — Output of `sudo nginx -t`

Add your screenshot here.

![Screenshot2](<Screenshot 2_assignment3_task2.png>)

---

#### Screenshot 3 — Output of `sudo ss -lptn '( sport = :80 )'`

Add your screenshot here.

![Screenshot3](<Screenshot 3_assignment_task2.png>)

---

### Notes

Answer the following in your own words:

**1. What happens if Nginx fails to restart in production?**

Write your answer here.

If Nginx fails to restart, the web server goes offline completely. Anyone trying to visit our EpicReads bookstore will see a "Site Can't Be Reached" error. Even though the application files are still on the server, there is no web server running to deliver them to visitors.

---

**2. What's your basic rollback plan?**

Write your answer here.

If Nginx crashes during a reload or restart, my quick recovery plan is to immediately check the configuration for errors using sudo nginx -t. If a typo broke it, I will restore the last known working backup file from /etc/nginx/sites-available/default and run sudo systemctl restart nginx to bring the site back online.

---

# Task 3 — Logs & Request Trace

## Goal

Verify real traffic flow and analyze logs to understand system behavior and errors.

### Evidence

#### Screenshot 1 — Output of `sudo tail -n 30 /var/log/nginx/access.log`

Add your screenshot here.

![Screenshot1](<Screenshot 1_assignment_task3.png>)

---

#### Screenshot 2 — Output of `sudo tail -n 30 /var/log/nginx/error.log`

Add your screenshot here.

![Screenshot2](<Screenshot 2_assignment3_task3.png>)

---

#### Screenshot 3 — Output of `sudo journalctl -u nginx --no-pager -n 50`

Add your screenshot here.

![Screenshot3](<Screenshot 3_assignment3_task3.png>)

---

### Notes

Answer the following in your own words:

**1. Were there any errors in the logs?**

- If yes, mention 1–2 example error lines from the logs and explain what each one means in simple terms.
- If no, explain what it means if the error log is empty or shows no recent errors during your check.

Write your answer here.

No, my error log file was clean. When the error log is empty, it means Nginx is running smoothly behind the scenes, and visitors are accessing our website files without Nginx running into any permission issues, missing files, or system crashes.

---

**2. If there were no errors, what does that indicate about the system?**

Write your answer here.

It shows that our server configuration is solid and healthy. It means Nginx has correct permissions to read our React build files, the paths are pointing to the right place, and the server isn't struggling with background crashes.

---

**3. Based on the access logs, were your curl requests visible in the log entries? What does that prove about traffic flow?**

Write your answer here.

Yes, I can see log entries displaying "curl" as the user-agent alongside my public IP address. This proves that traffic is flowing successfully all the way from my requests, past the cloud firewall, and directly into the Nginx web server where it is being logged in real-time.

---

# Task 4 — System Resource Health Check (Capacity Red Flags)

## Goal

Assess server capacity and detect potential performance or failure risks.

### Evidence

#### Screenshot 1 — Output of `uptime`

Add your screenshot here.

![Screenshot1](<Screenshot 1_assignment3_task4.png>)

---

#### Screenshot 2 — Output of `free -h`

Add your screenshot here.

![Screenshot2](<Screenshot 2_assignment3_task4.png>)

---

#### Screenshot 3 — Output of `df -h`

Add your screenshot here.

![Screenshot4](<Screenshot 3_assignment3_task4.png>)

---

#### Screenshot 4 — Output of `sudo du -sh /var/* | sort -h`

Add your screenshot here.

![Screenshot4](<Screenshot 4_assignment3_task4.png>)

---

### Notes

Answer the following in your own words:

**1. Which resource looks most critical right now? (CPU/load, memory, or disk) Explain why.**

Write your answer here.

On an AWS t2.micro instance, Memory (RAM) is usually the most critical resource. Since these servers only have 1 GB of RAM, running builds or having too many background tasks can easily exhaust it. My current free -h output shows that a large chunk of my RAM is active, meaning we have to be careful not to overload it with unnecessary software.

---

**2. What happens if disk becomes 100% full in a production server?**

Write your answer here.

If the hard drive becomes 100% full, the server will experience critical failures. Nginx won't be able to write access or error logs, database software won't be able to save new entries, and essential system processes will crash. In many cases, you won't even be able to SSH back into the machine because the server cannot create temporary session files.

---

# Task 5 — Configuration & Deployment Verification

## Goal

Ensure the correct React build is deployed and Nginx is serving it properly.

### Evidence

#### Screenshot 1 — Output of `ls -lah /var/www/html | head -n 20`

Add your screenshot here.

![Screenshot1](<Screenshot 1_assignment3_task5.png>)

---

#### Screenshot 2 — Output of `grep -R "Deployed by" -n /var/www/html 2>/dev/null | head`

Add your screenshot here.

![Screenshot2](<Screenshot 2_assignment3_task5.png>)

---

#### Screenshot 3 — Output of `grep -n "try_files" /etc/nginx/sites-available/default`

Add your screenshot here.

![Screenshot3](<Screenshot 3_assignment3_task5.png>)

---

### Notes

Answer the following in your own words:

**1. How do you confirm that the correct version of the application is deployed?**

Write your answer here.

I confirm the correct version is deployed by searching for my name directly in the deployed files using grep. Seeing my name ("David Agada Adikwu") in the static HTML or JavaScript files inside /var/www/html verifies that Nginx is serving my custom build rather than a default template.

---

# Task 6 — Nginx Configuration Failure Simulation

## Goal

Simulate a real-world Nginx misconfiguration and recover the service safely.

### Evidence

#### Screenshot 1 — Output of `sudo nginx -t` showing the syntax error (broken config)

Add your screenshot here.

![Screenshot1](<Screenshot 1_assignment3_task6.png>)

---

#### Screenshot 2 — Output of `sudo nginx -t` showing syntax ok (fixed config)

Add your screenshot here.

![Screenshot2](<Screenshot 2_assignment3_task6.png>)

---

#### Screenshot 3 — Output of `curl -I http://<public-ip>` confirming recovery (200 OK)

Add your screenshot here.

![Screenshot3](<Screenshot 3_assignment3_task6.png>)

---

### Notes

Answer the following in your own words:

**1. What caused the configuration failure?**

Write your answer here.

The failure was caused by a deliberate syntax error where a terminating semicolon ; was removed from a directive inside the Nginx configuration file. Nginx strictly requires semicolons to parse its rules; without them, the service fails to boot or reload.

---

**2. How did you fix the issue?**

Write your answer here.

I ran the syntax test tool (sudo nginx -t), which pointed out the exact line number and the missing character. I then opened the configuration file in nano, restored the missing semicolon, re-ran the test to verify it was fixed, and restarted Nginx.

---

**3. How can you avoid this kind of issue in real production systems?**

Write your answer here.

By always running nginx -t before restarting the service, using automation templates (like Ansible or Terraform) to deploy configurations, and keeping configuration files under Git version control to easily track and revert changes.

---

# Task 7 — Web Application Failure Simulation

## Goal

Simulate missing deployment content and recover the application safely.

### Evidence

#### Screenshot 1 — Output of `curl -I http://<public-ip>` showing failure (non-200 response)

Add your screenshot here.

![Screenshot1](<Screenshot 1_assignment3_task7.png>)

---

#### Screenshot 2 — Output of `curl -I http://<public-ip>` confirming recovery (200 OK)

Add your screenshot here.

![Screenshot2](<Screenshot 2_assignment3_task7.png>)

---

### Notes

Answer the following in your own words:

**1. What caused the application to break in this scenario?**

Write your answer here

The application broke because Nginx could no longer access the React build files inside /var/www/html. Because the directory was missing or empty, Nginx couldn't serve the default index.html file, returning a 403 Forbidden or 404 Not Found error to the client.

---

**2. How did you fix the issue and restore the application?**

Write your answer here.

I restored the application by returning the compiled React files back to the /var/www/html folder and ensuring that the web server user (www-data) had the correct permissions to read the directory.

---

**3. What steps would you take to prevent this kind of issue in real production systems?**

Write your answer here.

To prevent this, we should set up CI/CD pipelines that test builds before deploying them, perform automated backups of the web root directory, and implement system monitoring (like Prometheus or Datadog) to alert us instantly if a URL stops returning a 200 OK status.

---

# Task 8 — Security & Reliability Review

## Goal

Review and reflect on the security and reliability practices applied during this assignment.

### Security & Reliability Notes

Answer the following in your own words:

**1. Why is SSH key-based authentication more secure than sharing passwords?**

Write your answer here.

SSH keys use asymmetric cryptography (a public-private key pair) which is extremely long and impossible to brute-force or guess. Passwords, on the other hand, can be easily guessed, stolen by phishing, or intercepted if shared over non-secure communication channels.

---

**2. Why should only required ports be open on a production server?**

Write your answer here.

Opening only necessary ports minimizes the "attack surface" of your server. Every open port represents a potential doorway for malicious actors to try and exploit vulnerabilities in services running behind those ports. Keeping them closed by default keeps the system secure.

---

**3. Why is it important for Nginx to be enabled on boot?**

Write your answer here.

If the underlying server crashes, undergoes maintenance, or reboots, Nginx must start up automatically. If it isn't enabled on boot, the website will remain down after a reboot until an administrator manually logs in to start the service, leading to unnecessary downtime.

---

**4. What are the risks of sharing secrets, keys, or credentials publicly?**

Write your answer here.

Sharing secrets publicly (like committing AWS keys or private SSH keys to public GitHub repositories) allows anyone to scan and steal those credentials. Within minutes, malicious bots can use them to spin up expensive resources, steal sensitive database information, or compromise your entire cloud infrastructure.

---

**5. Why should cloud resources be stopped or terminated when they are no longer needed?**

Write your answer here.

Cloud resources operate on a pay-as-you-go model. Leaving unused virtual machines, databases, or disks running will continuously rack up charges on your account. Terminating them when they are no longer needed saves money and prevents waste.

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
- Do not expose sensitive information (keys, passwords, account IDs)

---

# Completion Checklist

- [ ] Task 1: Screenshots (browser, ip a, ss -tulpen, ufw status) + Notes answered
- [ ] Task 2: Screenshots (nginx status, nginx -t, ss port 80) + Notes answered
- [ ] Task 3: Screenshots (access log, error log, journalctl) + Notes answered
- [ ] Task 4: Screenshots (uptime, free -h, df -h, du -sh) + Notes answered
- [ ] Task 5: Screenshots (ls html, grep deployed by, grep try_files) + Notes answered
- [ ] Task 6: Screenshots (nginx -t fail, nginx -t pass, curl recovery) + Notes answered
- [ ] Task 7: Screenshots (curl failure, curl recovery) + Notes answered
- [ ] Task 8: Security & Reliability Notes answered
- [ ] LinkedIn post published and URL submitted
- [ ] Full Name visible in all required screenshots
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
