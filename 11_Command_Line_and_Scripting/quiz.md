# Command Line Mastery Quiz

**Basic Navigation and File Operations:**

1. **Directory Tree Navigation:** You're in `/home/user/projects` and need to get to `/var/log/apache2`. What command would you use, and explain the path type (absolute vs relative)?

2. **File Search Challenge:** Find all Python files in your home directory that contain the word "TODO" in comments, and display the matching lines with line numbers.

3. **Bulk File Operations:** You have 100 text files that need to be renamed from `file001.txt` to `document001.txt`. Write a command to do this efficiently.

**Text Processing and Analysis:**

4. **Log Analysis Pipeline:** Analyze a web server access log to find the top 10 IP addresses by request count, showing only the IPs and their counts.

5. **Data Extraction:** Extract all email addresses from a text file containing mixed content, removing duplicates and sorting alphabetically.

6. **CSV Processing:** Process a CSV file to calculate the average of a numeric column and count rows where another column meets a condition.

**Scripting Fundamentals:**

7. **Backup Script:** Write a bash script that creates timestamped backups of a directory, rotates old backups (keep only last 7), and logs the operation.

8. **System Monitoring:** Create a script that checks disk usage, CPU load, and memory usage, sending an email alert if any metric exceeds thresholds.

9. **Batch Processing:** Write a script that processes multiple image files, converting them to a different format using ImageMagick, with progress reporting and error handling.

**Git Workflow Scenarios:**

10. **Feature Development:** Walk through the complete Git workflow for developing a new feature, including branching, committing, pushing, and creating a pull request.

11. **Conflict Resolution:** During a merge, you encounter conflicts in multiple files. Demonstrate how you'd resolve them using command-line Git tools.

12. **Repository Cleanup:** Your repository has accumulated many stale branches and large binary files. How would you clean it up using Git commands?

**Networking and Remote Operations:**

13. **SSH Key Management:** Set up SSH key authentication for secure, passwordless access to a remote server, including key generation and server configuration.

14. **Remote File Synchronization:** Synchronize a local directory with a remote server, ensuring only changed files are transferred and handling network interruptions gracefully.

15. **Network Troubleshooting:** A service is unreachable. Use command-line tools to diagnose whether it's a DNS, connectivity, or service issue.

**Process and System Management:**

16. **Process Investigation:** A server is running slowly. Identify resource-intensive processes, check system load, and determine if any processes should be terminated.

17. **Service Management:** Configure a web server to start automatically on boot, monitor its status, and set up log rotation using command-line tools.

18. **Package Installation:** Install a complex software stack (like a LAMP server) using package managers, ensuring all dependencies are resolved and services start correctly.

**Security and Best Practices:**

19. **Secure File Operations:** Demonstrate secure file operations including encryption/decryption, secure deletion, and permission management for sensitive files.

20. **Audit Trail Creation:** Set up command logging and history tracking to maintain an audit trail of system administration activities.

21. **Vulnerability Assessment:** Use command-line tools to check for common security issues like weak permissions, exposed services, and outdated packages.

**Advanced Scripting:**

22. **API Integration Script:** Create a script that interacts with a REST API, handles authentication, processes JSON responses, and implements retry logic for failures.

23. **Configuration Management:** Write a script that manages application configuration across multiple environments, with validation and rollback capabilities.

24. **Automated Testing:** Develop a script that runs a test suite, parses results, generates reports, and notifies team members of failures.

**Container and Cloud CLI:**

25. **Docker Workflow:** Build a Docker image, run containers, manage volumes, and implement a multi-container application using Docker Compose.

26. **Cloud Resource Management:** Use AWS CLI to create EC2 instances, configure security groups, set up load balancers, and monitor resource usage.

**Performance and Monitoring:**

27. **System Performance Analysis:** Diagnose a performance issue by monitoring CPU, memory, disk I/O, and network usage over time, identifying bottlenecks.

28. **Log Monitoring:** Set up real-time log monitoring to alert on specific patterns, rotate logs, and archive old logs automatically.

**Integration and Automation:**

29. **Workflow Automation:** Create a complete automation workflow that pulls code from Git, runs tests, builds artifacts, and deploys to staging if successful.

30. **Cross-Platform Scripting:** Write a script that works on both Linux and macOS, handling platform differences gracefully.

**Troubleshooting Scenarios:**

31. **Debugging a Failing Script:** A complex script fails intermittently. How would you add comprehensive debugging, logging, and error handling?

32. **Performance Bottleneck:** An application runs slowly. Use command-line profiling tools to identify the bottleneck and suggest optimizations.

**Real-World Problem Solving:**

33. **Data Migration Script:** Write a script to migrate data from one database to another, with progress tracking, error recovery, and validation.

34. **Deployment Automation:** Create a deployment script that handles database migrations, service restarts, health checks, and rollback procedures.

35. **Infrastructure Setup:** Automate the setup of a development environment, including installing dependencies, configuring services, and setting up databases.