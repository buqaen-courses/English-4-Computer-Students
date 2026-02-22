# Command Line Mastery: Solutions & Best Practices

**Basic Navigation and File Operations Solutions:**

1. **Directory Tree Navigation**
   - **Command:** `cd /var/log/apache2`
   - **Path Type:** Absolute path (starts with `/`, represents complete location from root)
   - **Alternative:** If currently in `/home/user/projects`, you could use `cd ../../../var/log/apache2` (relative path)

2. **File Search Challenge**
   ```bash
   find ~ -name "*.py" -type f -exec grep -Hn "TODO" {} \;
   ```
   - `find ~ -name "*.py"` finds all Python files in home directory
   - `-type f` ensures only files, not directories
   - `-exec grep -Hn "TODO" {} \;` searches each file for "TODO" with line numbers

3. **Bulk File Operations**
   ```bash
   for file in file*.txt; do mv "$file" "${file/file/document}"; done
   ```
   - Or using rename command: `rename 's/^file/document/' file*.txt`
   - Or using mmv: `mmv 'file*.txt' 'document#1.txt'`

**Text Processing and Analysis Solutions:**

4. **Log Analysis Pipeline**
   ```bash
   awk '{print $1}' access.log | sort | uniq -c | sort -nr | head -10
   ```
   - `awk '{print $1}'` extracts first field (IP address, assuming standard log format)
   - `sort | uniq -c` counts occurrences
   - `sort -nr` sorts by count descending
   - `head -10` shows top 10

5. **Data Extraction**
   ```bash
   grep -E '\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b' file.txt | sort | uniq
   ```
   - Uses regex pattern to match email addresses
   - `sort | uniq` removes duplicates and sorts

6. **CSV Processing**
   ```bash
   # Calculate average of column 3 and count rows where column 2 > 100
   awk -F',' 'NR>1 {sum+=$3; count++} $2>100 {above++} END {print "Average:", sum/count; print "Rows above 100:", above}' data.csv
   ```
   - `NR>1` skips header row
   - Accumulates sum and count for average
   - Counts rows meeting condition

**Scripting Fundamentals Solutions:**

7. **Backup Script**
   ```bash
   #!/bin/bash
   SOURCE_DIR="/path/to/source"
   BACKUP_DIR="/path/to/backups"
   TIMESTAMP=$(date +%Y%m%d_%H%M%S)
   
   # Create backup
   tar -czf "${BACKUP_DIR}/backup_${TIMESTAMP}.tar.gz" "$SOURCE_DIR"
   
   # Rotate old backups (keep last 7)
   ls -t "${BACKUP_DIR}"/backup_*.tar.gz | tail -n +8 | xargs rm -f
   
   # Log operation
   echo "$(date): Backup completed - ${BACKUP_DIR}/backup_${TIMESTAMP}.tar.gz" >> "${BACKUP_DIR}/backup.log"
   ```

8. **System Monitoring**
   ```bash
   #!/bin/bash
   ALERT_EMAIL="admin@example.com"
   DISK_THRESHOLD=90
   CPU_THRESHOLD=80
   MEM_THRESHOLD=85
   
   # Check disk usage
   DISK_USAGE=$(df / | tail -1 | awk '{print $5}' | sed 's/%//')
   if [ "$DISK_USAGE" -gt "$DISK_THRESHOLD" ]; then
       echo "Disk usage is ${DISK_USAGE}%" | mail -s "Disk Alert" "$ALERT_EMAIL"
   fi
   
   # Check CPU load
   CPU_LOAD=$(uptime | awk -F'load average:' '{print $2}' | awk '{print $1}' | sed 's/,//')
   if (( $(echo "$CPU_LOAD > $CPU_THRESHOLD" | bc -l) )); then
       echo "CPU load is ${CPU_LOAD}" | mail -s "CPU Alert" "$ALERT_EMAIL"
   fi
   
   # Add similar check for memory usage
   ```

9. **Batch Processing**
   ```bash
   #!/bin/bash
   SOURCE_DIR="/path/to/images"
   OUTPUT_DIR="/path/to/converted"
   TOTAL=$(ls "$SOURCE_DIR"/*.jpg 2>/dev/null | wc -l)
   COUNT=0
   
   mkdir -p "$OUTPUT_DIR"
   
   for image in "$SOURCE_DIR"/*.jpg; do
       if [ -f "$image" ]; then
           filename=$(basename "$image" .jpg)
           output="${OUTPUT_DIR}/${filename}.png"
           
           if convert "$image" "$output" 2>/dev/null; then
               echo "Converted: $filename.jpg -> $filename.png"
               ((COUNT++))
               echo "Progress: $COUNT/$TOTAL"
           else
               echo "Error converting: $filename.jpg" >&2
           fi
       fi
   done
   
   echo "Conversion complete: $COUNT/$TOTAL images processed"
   ```

**Git Workflow Scenarios Solutions:**

10. **Feature Development**
   ```bash
   # Create and switch to feature branch
   git checkout -b feature/user-authentication
   
   # Make changes and commit
   git add .
   git commit -m "Add basic user authentication with JWT"
   
   # Push branch to remote
   git push -u origin feature/user-authentication
   
   # Create pull request via GitHub/GitLab interface
   # After review and approval:
   git checkout main
   git pull origin main
   git merge feature/user-authentication
   git push origin main
   git branch -d feature/user-authentication
   git push origin --delete feature/user-authentication
   ```

11. **Conflict Resolution**
   ```bash
   # During merge attempt
   git status  # See conflicted files
   
   # Edit conflicted files manually, choosing which changes to keep
   # For each conflict, choose <<<<HEAD, ======, >>>>>branch markers
   
   # After resolving conflicts
   git add resolved_file.txt
   git commit -m "Resolve merge conflicts in user authentication"
   
   # Alternative: Use mergetool
   git mergetool
   git commit -m "Resolve merge conflicts with mergetool"
   ```

12. **Repository Cleanup**
   ```bash
   # Remove merged branches
   git branch --merged | grep -v "main\|master\|develop" | xargs git branch -d
   
   # Remove large files with BFG Repo-Cleaner or git-filter-repo
   # (Install BFG first)
   java -jar bfg.jar --strip-blobs-bigger-than 100M .
   
   # Clean up reflog and repack
   git reflog expire --expire=now --all
   git gc --prune=now --aggressive
   
   # Remove untracked files
   git clean -fd
   ```

**Networking and Remote Operations Solutions:**

13. **SSH Key Management**
   ```bash
   # Generate SSH key pair
   ssh-keygen -t ed25519 -C "your_email@example.com"
   
   # Copy public key to server
   ssh-copy-id user@hostname
   
   # Or manually append to authorized_keys
   cat ~/.ssh/id_ed25519.pub | ssh user@hostname "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
   
   # Set proper permissions
   ssh user@hostname "chmod 700 ~/.ssh && chmod 600 ~/.ssh/authorized_keys"
   
   # Test connection
   ssh user@hostname
   ```

14. **Remote File Synchronization**
   ```bash
   # Basic sync
   rsync -avz --delete local_dir/ user@remote:/path/to/remote_dir/
   
   # With progress and resume capability
   rsync -avzP --delete --partial local_dir/ user@remote:/path/to/remote_dir/
   
   # Exclude certain files
   rsync -avz --exclude '*.tmp' --exclude 'cache/' local_dir/ user@remote:/path/to/remote_dir/
   
   # Dry run first
   rsync -avzn local_dir/ user@remote:/path/to/remote_dir/
   ```

15. **Network Troubleshooting**
   ```bash
   # Check local connectivity
   ping website.com
   
   # Check DNS resolution
   nslookup website.com
   dig website.com
   
   # Trace network path
   traceroute website.com
   
   # Check if port is open
   telnet website.com 80
   nc -zv website.com 80
   
   # Check local firewall
   sudo ufw status
   sudo iptables -L
   
   # Check routing
   ip route show
   netstat -rn
   ```

**Process and System Management Solutions:**

16. **Process Investigation**
   ```bash
   # Check system load
   uptime
   top -b -n1 | head -20
   
   # Find high CPU processes
   ps aux --sort=-%cpu | head -10
   
   # Find high memory processes
   ps aux --sort=-%mem | head -10
   
   # Check specific process details
   ps -p PID -o pid,ppid,cmd,%cpu,%mem,start,time
   
   # Kill problematic process (use with caution)
   kill -15 PID  # Graceful shutdown
   kill -9 PID   # Force kill if needed
   ```

17. **Service Management**
   ```bash
   # Install and configure Apache (Ubuntu/Debian)
   sudo apt update
   sudo apt install apache2
   
   # Enable auto-start
   sudo systemctl enable apache2
   
   # Start service
   sudo systemctl start apache2
   
   # Check status
   sudo systemctl status apache2
   
   # Configure log rotation
   sudo nano /etc/logrotate.d/apache2
   # Add configuration for /var/log/apache2/*.log
   
   # Reload logrotate
   sudo logrotate -f /etc/logrotate.d/apache2
   ```

18. **Package Installation**
   ```bash
   # Update package list
   sudo apt update
   
   # Install LAMP stack
   sudo apt install apache2 mysql-server php libapache2-mod-php php-mysql
   
   # Verify installations
   apache2 -v
   mysql --version
   php -v
   
   # Configure services
   sudo systemctl enable apache2
   sudo systemctl enable mysql
   sudo systemctl start apache2
   sudo systemctl start mysql
   
   # Secure MySQL installation
   sudo mysql_secure_installation
   
   # Test PHP
   echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/test.php
   curl localhost/test.php
   ```

**Security and Best Practices Solutions:**

19. **Secure File Operations**
   ```bash
   # Encrypt file
   openssl enc -aes-256-cbc -salt -in sensitive.txt -out sensitive.enc
   
   # Decrypt file
   openssl enc -d -aes-256-cbc -in sensitive.enc -out sensitive.txt
   
   # Secure delete (multiple passes)
   shred -u -z -n 3 sensitive.txt
   
   # Set secure permissions
   chmod 600 private_key.pem
   chown root:root private_key.pem
   
   # Check file integrity
   sha256sum file.txt > file.txt.sha256
   sha256sum -c file.txt.sha256
   ```

20. **Audit Trail Creation**
   ```bash
   # Enable command history
   export HISTSIZE=10000
   export HISTFILESIZE=20000
   shopt -s histappend  # Append to history file
   
   # Add timestamps to history
   export HISTTIMEFORMAT="%F %T "
   
   # Log all commands (add to .bashrc)
   export PROMPT_COMMAND='echo "$(date +%Y-%m-%d_%H:%M:%S) $(whoami) $(pwd) $(history 1 | sed "s/^[ ]*[0-9]*[ ]*//")" >> ~/.command_log'
   
   # For system-wide logging, configure syslog
   sudo nano /etc/rsyslog.d/command-audit.conf
   # Add: local6.*    /var/log/commands.log
   ```

21. **Vulnerability Assessment**
   ```bash
   # Check for weak file permissions
   find /home -type f -perm 777 2>/dev/null
   find /etc -name "*.conf" -perm /o+r 2>/dev/null
   
   # Check for exposed services
   netstat -tlnp | grep -v 127.0.0.1
   ss -tlnp | grep LISTEN
   
   # Check for outdated packages
   apt list --upgradable
   yum check-update
   
   # Check running services
   systemctl list-units --type=service --state=running
   service --status-all | grep running
   
   # Use automated tools
   sudo lynis audit system
   sudo rkhunter --check
   ```

**Advanced Scripting Solutions:**

22. **API Integration Script**
   ```bash
   #!/bin/bash
   API_URL="https://api.example.com"
   API_KEY="your-api-key"
   MAX_RETRIES=3
   
   call_api() {
       local endpoint=$1
       local method=${2:-GET}
       local data=$3
       local retry_count=0
       
       while [ $retry_count -lt $MAX_RETRIES ]; do
           response=$(curl -s -X "$method" \
               -H "Authorization: Bearer $API_KEY" \
               -H "Content-Type: application/json" \
               ${data:+-d "$data"} \
               "$API_URL$endpoint")
           
           if [ $? -eq 0 ]; then
               echo "$response"
               return 0
           fi
           
           ((retry_count++))
           sleep $((2 ** retry_count))
       done
       
       echo "API call failed after $MAX_RETRIES retries" >&2
       return 1
   }
   
   # Usage
   result=$(call_api "/users" "POST" '{"name":"John","email":"john@example.com"}')
   if [ $? -eq 0 ]; then
       echo "Success: $result"
   else
       echo "Failed to create user"
       exit 1
   fi
   ```

23. **Configuration Management**
   ```bash
   #!/bin/bash
   CONFIG_FILE="app.config"
   BACKUP_DIR="config_backups"
   
   validate_config() {
       local file=$1
       
       # Check required fields
       if ! grep -q "^database_url=" "$file"; then
           echo "Error: database_url is required"
           return 1
       fi
       
       if ! grep -q "^port=[0-9]\+$" "$file"; then
           echo "Error: port must be a number"
           return 1
       fi
       
       return 0
   }
   
   deploy_config() {
       local env=$1
       local source_config="config/$env.config"
       local target_config="app.config"
       
       if [ ! -f "$source_config" ]; then
           echo "Config file for $env not found"
           return 1
       fi
       
       # Backup current config
       mkdir -p "$BACKUP_DIR"
       cp "$target_config" "$BACKUP_DIR/$(basename "$target_config").$(date +%Y%m%d_%H%M%S).bak" 2>/dev/null || true
       
       # Validate new config
       if ! validate_config "$source_config"; then
           echo "Config validation failed"
           return 1
       fi
       
       # Deploy config
       cp "$source_config" "$target_config"
       echo "Config deployed successfully for $env"
       
       # Restart application if needed
       systemctl restart myapp
   }
   
   # Usage: ./deploy_config.sh production
   deploy_config "$1"
   ```

24. **Automated Testing**
   ```bash
   #!/bin/bash
   TEST_DIR="tests"
   REPORT_FILE="test_report_$(date +%Y%m%d_%H%M%S).txt"
   EMAIL_RECIPIENTS="team@example.com"
   
   run_tests() {
       echo "Starting test suite at $(date)" > "$REPORT_FILE"
       echo "==================================" >> "$REPORT_FILE"
       
       local total_tests=0
       local passed_tests=0
       local failed_tests=0
       
       for test_file in "$TEST_DIR"/*.test; do
           if [ -f "$test_file" ]; then
               ((total_tests++))
               echo "Running $(basename "$test_file")..." | tee -a "$REPORT_FILE"
               
               if bash "$test_file" >> "$REPORT_FILE" 2>&1; then
                   ((passed_tests++))
                   echo "✓ PASSED" | tee -a "$REPORT_FILE"
               else
                   ((failed_tests++))
                   echo "✗ FAILED" | tee -a "$REPORT_FILE"
               fi
               echo "---" >> "$REPORT_FILE"
           fi
       done
       
       echo "==================================" >> "$REPORT_FILE"
       echo "Summary: $passed_tests/$total_tests tests passed" >> "$REPORT_FILE"
       echo "Failed tests: $failed_tests" >> "$REPORT_FILE"
       
       return $failed_tests
   }
   
   send_notification() {
       local subject="Test Results: $(basename "$REPORT_FILE")"
       
       if [ $1 -eq 0 ]; then
           subject="✅ All Tests Passed - $subject"
       else
           subject="❌ Test Failures Detected - $subject"
       fi
       
       mail -s "$subject" -A "$REPORT_FILE" "$EMAIL_RECIPIENTS" < /dev/null
   }
   
   # Run tests
   run_tests
   test_result=$?
   
   # Send notification
   send_notification $test_result
   
   # Exit with test result
   exit $test_result
   ```

**Container and Cloud CLI Solutions:**

25. **Docker Workflow**
   ```bash
   # Create Dockerfile
   cat > Dockerfile << EOF
   FROM node:16-alpine
   WORKDIR /app
   COPY package*.json ./
   RUN npm ci --only=production
   COPY . .
   EXPOSE 3000
   CMD ["npm", "start"]
   EOF
   
   # Build image
   docker build -t my-web-app .
   
   # Run container
   docker run -d -p 3000:3000 --name web-app my-web-app
   
   # Create docker-compose.yml for multi-container
   cat > docker-compose.yml << EOF
   version: '3.8'
   services:
     web:
       build: .
       ports:
         - "3000:3000"
     db:
       image: postgres:13
       environment:
         POSTGRES_PASSWORD: mypassword
       volumes:
         - db_data:/var/lib/postgresql/data
   volumes:
     db_data:
   EOF
   
   # Run multi-container app
   docker-compose up -d
   ```

26. **Cloud Resource Management**
   ```bash
   # Configure AWS CLI (first time setup)
   aws configure
   
   # Create VPC
   VPC_ID=$(aws ec2 create-vpc --cidr-block 10.0.0.0/16 --query 'Vpc.VpcId' --output text)
   
   # Create subnet
   SUBNET_ID=$(aws ec2 create-subnet --vpc-id $VPC_ID --cidr-block 10.0.1.0/24 --query 'Subnet.SubnetId' --output text)
   
   # Create security group
   SG_ID=$(aws ec2 create-security-group --group-name my-sg --description "My security group" --vpc-id $VPC_ID --query 'GroupId' --output text)
   
   # Add inbound rules
   aws ec2 authorize-security-group-ingress --group-id $SG_ID --protocol tcp --port 80 --cidr 0.0.0.0/0
   aws ec2 authorize-security-group-ingress --group-id $SG_ID --protocol tcp --port 443 --cidr 0.0.0.0/0
   aws ec2 authorize-security-group-ingress --group-id $SG_ID --protocol tcp --port 22 --cidr 0.0.0.0/0
   
   # Launch EC2 instance
   INSTANCE_ID=$(aws ec2 run-instances --image-id ami-12345678 --count 1 --instance-type t2.micro --key-name my-key --security-group-ids $SG_ID --subnet-id $SUBNET_ID --query 'Instances[0].InstanceId' --output text)
   
   # Create Application Load Balancer
   ALB_ARN=$(aws elbv2 create-load-balancer --name my-alb --subnets $SUBNET_ID --security-groups $SG_ID --query 'LoadBalancers[0].LoadBalancerArn' --output text)
   
   # Check instance status
   aws ec2 describe-instances --instance-ids $INSTANCE_ID --query 'Reservations[0].Instances[0].State.Name'
   
   # Monitor costs
   aws ce get-cost-and-usage --time-period Start=2024-01-01,End=2024-01-31 --granularity MONTHLY --metrics "BlendedCost"
   ```

**Performance and Monitoring Solutions:**

27. **System Performance Analysis**
   ```bash
   # Overall system stats
   uptime
   free -h
   df -h
   
   # CPU analysis
   top -b -n1 | head -20
   iostat -c 1 5  # CPU utilization over time
   
   # Memory analysis
   vmstat 1 5     # Memory stats over time
   ps aux --sort=-%mem | head -10  # Memory-hungry processes
   
   # Disk I/O analysis
   iostat -d 1 5  # Disk I/O stats
   iotop          # Real-time I/O monitoring
   
   # Network analysis
   iftop          # Network bandwidth usage
   netstat -i     # Network interface stats
   ss -tuln       # Listening ports and connections
   
   # Process-specific analysis
   pidstat -p PID 1 5  # Per-process CPU/memory stats
   strace -p PID -c    # System call analysis
   ```

28. **Log Monitoring**
   ```bash
   # Real-time log monitoring with alerts
   tail -f /var/log/application.log | grep --line-buffered "ERROR" | while read line; do
       echo "$(date): $line"
       # Send alert
       curl -X POST -H 'Content-type: application/json' \
       --data "{\"text\":\"Error detected: $line\"}" \
       https://hooks.slack.com/services/YOUR/SLACK/WEBHOOK
   done
   
   # Log rotation setup
   cat > /etc/logrotate.d/myapp << EOF
   /var/log/myapp/*.log {
       daily
       rotate 30
       compress
       missingok
       notifempty
       create 644 myapp myapp
       postrotate
           systemctl reload myapp
       endscript
   }
   EOF
   
   # Archive old logs
   find /var/log/myapp -name "*.log.*.gz" -mtime +30 -delete
   ```

**Integration and Automation Solutions:**

29. **Workflow Automation**
   ```bash
   #!/bin/bash
   set -e  # Exit on any error
   
   echo "Starting deployment pipeline..."
   
   # Pull latest code
   git pull origin main
   
   # Install dependencies
   npm ci
   
   # Run tests
   npm test
   
   # Build application
   npm run build
   
   # Run integration tests
   npm run test:integration
   
   # Deploy to staging
   echo "Deploying to staging..."
   scp -r dist/* staging-server:/var/www/html/
   
   # Run smoke tests on staging
   curl -f http://staging-server/health || exit 1
   
   echo "✅ Pipeline completed successfully!"
   echo "🚀 Ready for production deployment"
   ```

30. **Cross-Platform Scripting**
   ```bash
   #!/bin/bash
   # Cross-platform script for Linux and macOS
   
   # Detect OS
   if [[ "$OSTYPE" == "linux-gnu"* ]]; then
       OS="linux"
       PACKAGE_MANAGER="apt"
   elif [[ "$OSTYPE" == "darwin"* ]]; then
       OS="macos"
       PACKAGE_MANAGER="brew"
   else
       echo "Unsupported OS: $OSTYPE"
       exit 1
   fi
   
   echo "Detected OS: $OS"
   
   # Cross-platform command execution
   get_date() {
       if [[ "$OS" == "macos" ]]; then
           date -u +"%Y-%m-%dT%H:%M:%SZ"
       else
           date --utc +"%Y-%m-%dT%H:%M:%SZ"
       fi
   }
   
   # Cross-platform file operations
   create_backup() {
       local file="$1"
       local backup="${file}.$(get_date).bak"
       
       if [[ "$OS" == "macos" ]]; then
           cp "$file" "$backup"
       else
           cp --backup=numbered "$file" "$backup"
       fi
       
       echo "Backup created: $backup"
   }
   
   # Cross-platform package installation
   install_package() {
       local package="$1"
       
       case $PACKAGE_MANAGER in
           apt)
               sudo apt update && sudo apt install -y "$package"
               ;;
           brew)
               brew install "$package"
               ;;
           *)
               echo "Unknown package manager: $PACKAGE_MANAGER"
               return 1
               ;;
       esac
   }
   
   # Usage examples
   create_backup "important_file.txt"
   install_package "curl"
   ```

**Troubleshooting Scenarios Solutions:**

31. **Debugging a Failing Script**
   ```bash
   # Add debugging to script
   #!/bin/bash
   set -x  # Print each command before execution
   set -e  # Exit on first error
   set -u  # Exit on undefined variables
   
   # Enhanced logging
   LOG_FILE="/tmp/script_debug.log"
   exec > >(tee -a "$LOG_FILE") 2>&1
   
   log() {
       echo "$(date +'%Y-%m-%d %H:%M:%S') [$$] $*" >&2
   }
   
   error_exit() {
       local line_no=$1
       local error_code=$2
       log "Error on line $line_no: Command exited with code $error_code"
       exit $error_code
   }
   
   trap 'error_exit ${LINENO} $?' ERR
   
   # Add checkpoints throughout script
   log "Starting script execution"
   
   # Before risky operations
   if [ ! -f "required_file.txt" ]; then
       log "ERROR: Required file missing"
       exit 1
   fi
   
   log "File check passed"
   
   # Use verbose flags where available
   curl -v https://api.example.com/endpoint
   
   log "Script completed successfully"
   ```

32. **Performance Bottleneck**
   ```bash
   # System-level analysis
   uptime                    # System load
   free -h                   # Memory usage
   df -h                     # Disk space
   iostat -x 1 3            # Disk I/O stats
   sar -u 1 3               # CPU usage history
   sar -r 1 3               # Memory usage history
   
   # Process-level analysis
   ps aux --sort=-%cpu | head -10  # CPU hogs
   ps aux --sort=-%mem | head -10  # Memory hogs
   pidstat -p PID 1 5             # Specific process monitoring
   
   # Application-level analysis
   top -p PID -d 1               # Process-specific top
   strace -p PID -c -S calls     # System call analysis
   lsof -p PID | head -20        # Open file analysis
   
   # Network analysis
   netstat -tuln | grep LISTEN   # Listening ports
   ss -tuln                      # Socket statistics
   iftop -i eth0                 # Network bandwidth
   
   # Database analysis (if applicable)
   mysqladmin processlist        # Active queries
   mysqldumpslow /var/log/mysql/slow.log  # Slow queries
   
   # Code-level analysis
   perf record -F 99 -p PID -- sleep 30  # CPU profiling
   perf report                             # Analyze results
   ```

**Real-World Problem Solving Solutions:**

33. **Data Migration Script**
   ```bash
   #!/bin/bash
   SOURCE_DB="mysql://user:pass@old-host/db"
   TARGET_DB="postgresql://user:pass@new-host/db"
   BATCH_SIZE=1000
   LOG_FILE="migration_$(date +%Y%m%d_%H%M%S).log"
   
   log() {
       echo "$(date +'%Y-%m-%d %H:%M:%S'): $*" | tee -a "$LOG_FILE"
   }
   
   error_exit() {
       log "ERROR: Migration failed at line $1"
       # Attempt rollback if needed
       log "Attempting rollback..."
       # Rollback logic here
       exit 1
   }
   
   trap 'error_exit $LINENO' ERR
   
   log "Starting data migration"
   
   # Get total record count
   TOTAL_RECORDS=$(mysql "$SOURCE_DB" -e "SELECT COUNT(*) FROM users" | tail -1)
   log "Total records to migrate: $TOTAL_RECORDS"
   
   # Migrate in batches
   OFFSET=0
   MIGRATED=0
   
   while [ $OFFSET -lt $TOTAL_RECORDS ]; do
       log "Processing batch: $OFFSET to $((OFFSET + BATCH_SIZE))"
       
       # Export batch from source
       mysql "$SOURCE_DB" -e "SELECT * FROM users LIMIT $BATCH_SIZE OFFSET $OFFSET" > temp_batch.csv
       
       # Transform data if needed (format differences, etc.)
       # Data transformation logic here
       
       # Import to target
       psql "$TARGET_DB" -c "\COPY users FROM 'temp_batch.csv' WITH CSV HEADER"
       
       # Validate batch
       IMPORTED_COUNT=$(psql "$TARGET_DB" -t -c "SELECT COUNT(*) FROM users")
       EXPECTED_COUNT=$((MIGRATED + BATCH_SIZE))
       
       if [ "$IMPORTED_COUNT" -lt "$EXPECTED_COUNT" ]; then
           log "ERROR: Import validation failed. Expected: $EXPECTED_COUNT, Got: $IMPORTED_COUNT"
           exit 1
       fi
       
       MIGRATED=$((MIGRATED + BATCH_SIZE))
       OFFSET=$((OFFSET + BATCH_SIZE))
       
       # Progress reporting
       PERCENT=$((MIGRATED * 100 / TOTAL_RECORDS))
       log "Progress: $MIGRATED/$TOTAL_RECORDS ($PERCENT%)"
       
       # Clean up temp files
       rm -f temp_batch.csv
   done
   
   log "Migration completed successfully!"
   log "Final validation..."
   
   # Final consistency check
   SOURCE_COUNT=$(mysql "$SOURCE_DB" -e "SELECT COUNT(*) FROM users" | tail -1)
   TARGET_COUNT=$(psql "$TARGET_DB" -t -c "SELECT COUNT(*) FROM users")
   
   if [ "$SOURCE_COUNT" -eq "$TARGET_COUNT" ]; then
       log "✅ Migration successful: $TARGET_COUNT records migrated"
   else
       log "❌ Migration validation failed: Source=$SOURCE_COUNT, Target=$TARGET_COUNT"
       exit 1
   fi
   ```

34. **Deployment Automation**
   ```bash
   #!/bin/bash
   set -e
   
   APP_NAME="my-web-app"
   DEPLOY_ENV="production"
   BACKUP_DIR="/opt/backups"
   APP_DIR="/opt/$APP_NAME"
   
   log() {
       echo "$(date +'%Y-%m-%d %H:%M:%S') [$DEPLOY_ENV]: $*"
   }
   
   error_exit() {
       log "❌ DEPLOYMENT FAILED: $*"
       # Trigger rollback
       rollback_deployment
       exit 1
   }
   
   create_backup() {
       log "📦 Creating backup..."
       TIMESTAMP=$(date +%Y%m%d_%H%M%S)
       BACKUP_FILE="$BACKUP_DIR/${APP_NAME}_${TIMESTAMP}.tar.gz"
       
       tar -czf "$BACKUP_FILE" -C "$APP_DIR" . || error_exit "Backup creation failed"
       log "✅ Backup created: $BACKUP_FILE"
   }
   
   run_database_migrations() {
       log "🗄️ Running database migrations..."
       
       # Backup database
       mysqldump -u dbuser -p dbpass myapp > "$BACKUP_DIR/db_backup_$TIMESTAMP.sql"
       
       # Run migrations
       cd "$APP_DIR"
       ./bin/migrate up || error_exit "Database migration failed"
       
       log "✅ Database migrations completed"
   }
   
   deploy_application() {
       log "🚀 Deploying application..."
       
       # Pull latest code
       git pull origin main || error_exit "Git pull failed"
       
       # Install dependencies
       npm ci || error_exit "Dependency installation failed"
       
       # Build application
       npm run build || error_exit "Build failed"
       
       # Run tests
       npm test || error_exit "Tests failed"
       
       log "✅ Application deployed successfully"
   }
   
   restart_services() {
       log "🔄 Restarting services..."
       
       # Graceful restart
       sudo systemctl reload "$APP_NAME" || sudo systemctl restart "$APP_NAME"
       
       # Wait for service to be healthy
       sleep 10
       
       # Health check
       curl -f http://localhost/health || error_exit "Health check failed"
       
       log "✅ Services restarted and healthy"
   }
   
   rollback_deployment() {
       log "🔙 Initiating rollback..."
       
       # Restore from backup
       cd "$APP_DIR"
       rm -rf *
       tar -xzf "$BACKUP_DIR/$(ls -t $BACKUP_DIR/${APP_NAME}_*.tar.gz | head -1)" -C .
       
       # Restore database if needed
       if [ -f "$BACKUP_DIR/db_backup_$TIMESTAMP.sql" ]; then
           mysql -u dbuser -p dbpass myapp < "$BACKUP_DIR/db_backup_$TIMESTAMP.sql"
       fi
       
       restart_services
       log "✅ Rollback completed"
   }
   
   run_health_checks() {
       log "🏥 Running health checks..."
       
       # Response time check
       RESPONSE_TIME=$(curl -o /dev/null -s -w '%{time_total}' http://localhost/)
       if (( $(echo "$RESPONSE_TIME > 2.0" | bc -l) )); then
           error_exit "Response time too slow: ${RESPONSE_TIME}s"
       fi
       
       # Error rate check
       ERROR_RATE=$(curl -s http://localhost/metrics | grep error_rate | awk '{print $2}')
       if (( $(echo "$ERROR_RATE > 0.05" | bc -l) )); then
           error_exit "Error rate too high: ${ERROR_RATE}"
       fi
       
       log "✅ All health checks passed"
   }
   
   # Main deployment process
   log "🚀 Starting deployment of $APP_NAME to $DEPLOY_ENV"
   
   create_backup
   run_database_migrations
   deploy_application
   restart_services
   run_health_checks
   
   log "🎉 Deployment completed successfully!"
   
   # Send notification
   curl -X POST -H 'Content-type: application/json' \
   --data "{\"text\":\"✅ $APP_NAME deployed successfully to $DEPLOY_ENV\"}" \
   https://hooks.slack.com/services/YOUR/WEBHOOK
   ```

35. **Infrastructure Setup**
   ```bash
   #!/bin/bash
   set -e
   
   # Configuration
   PROJECT_NAME="my-dev-env"
   NODE_VERSION="18"
   PYTHON_VERSION="3.9"
   DATABASE="postgresql"
   
   log() {
       echo "$(date +'%Y-%m-%d %H:%M:%S'): $*"
   }
   
   error_exit() {
       log "❌ Setup failed: $*"
       # Cleanup on failure
       cleanup_partial_install
       exit 1
   }
   
   check_os() {
       if [[ "$OSTYPE" == "linux-gnu"* ]]; then
           OS="linux"
           PACKAGE_MANAGER="apt"
       elif [[ "$OSTYPE" == "darwin"* ]]; then
           OS="macos"
           PACKAGE_MANAGER="brew"
       else
           error_exit "Unsupported OS: $OSTYPE"
       fi
       log "Detected OS: $OS with $PACKAGE_MANAGER"
   }
   
   install_system_dependencies() {
       log "📦 Installing system dependencies..."
       
       case $PACKAGE_MANAGER in
           apt)
               sudo apt update
               sudo apt install -y build-essential curl wget git vim htop tree
               ;;
           brew)
               brew update
               brew install curl wget git vim htop tree
               ;;
       esac
   }
   
   install_nodejs() {
       log "📦 Installing Node.js $NODE_VERSION..."
       
       # Install nvm (Node Version Manager)
       curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
       source ~/.bashrc
       
       # Install Node.js
       nvm install $NODE_VERSION
       nvm use $NODE_VERSION
       nvm alias default $NODE_VERSION
       
       # Verify installation
       node --version
       npm --version
   }
   
   install_python() {
       log "📦 Installing Python $PYTHON_VERSION..."
       
       case $PACKAGE_MANAGER in
           apt)
               sudo apt install -y python3 python3-pip python3-venv
               ;;
           brew)
               brew install python@$PYTHON_VERSION
               ;;
       esac
       
       # Install virtualenv
       pip3 install virtualenv
       
       # Verify installation
       python3 --version
       pip3 --version
   }
   
   install_database() {
       log "📦 Installing $DATABASE..."
       
       case $PACKAGE_MANAGER in
           apt)
               sudo apt install -y postgresql postgresql-contrib
               sudo systemctl enable postgresql
               sudo systemctl start postgresql
               
               # Create default database and user
               sudo -u postgres createuser --createdb --superuser $USER
               createdb $USER
               ;;
           brew)
               brew install postgresql
               brew services start postgresql
               
               # Initialize and create default user
               initdb /usr/local/var/postgres
               createdb $USER
               ;;
       esac
       
       # Verify installation
       psql --version
   }
   
   setup_project_structure() {
       log "📁 Setting up project structure..."
       
       # Create project directories
       mkdir -p ~/$PROJECT_NAME/{src,tests,docs,scripts}
       cd ~/$PROJECT_NAME
       
       # Initialize Git repository
       git init
       git config user.name "Developer"
       git config user.email "developer@example.com"
       
       # Create basic project files
       cat > README.md << EOF
   # $PROJECT_NAME
   
   Development environment setup complete!
   
   ## Quick Start
   
   1. Activate Python virtual environment:
      source venv/bin/activate
   
   2. Install dependencies:
      pip install -r requirements.txt
   
   3. Start development server:
      npm start
   EOF
   
       # Create requirements.txt
       cat > requirements.txt << EOF
   Flask==2.3.3
   requests==2.31.0
   pytest==7.4.0
   EOF
   
       # Create package.json
       cat > package.json << EOF
   {
     "name": "$PROJECT_NAME",
     "version": "1.0.0",
     "scripts": {
       "start": "node src/app.js",
       "test": "jest",
       "dev": "nodemon src/app.js"
     }
   }
   EOF
   
       # Create basic Node.js app
       mkdir -p src
       cat > src/app.js << EOF
   const express = require('express');
   const app = express();
   const port = 3000;
   
   app.get('/', (req, res) => {
     res.json({ message: 'Hello from $PROJECT_NAME!' });
   });
   
   app.listen(port, () => {
     console.log(\`$PROJECT_NAME listening at http://localhost:\${port}\`);
   });
   EOF
   }
   
   setup_environments() {
       log "🔧 Setting up development environments..."
       
       cd ~/$PROJECT_NAME
       
       # Python virtual environment
       python3 -m venv venv
       source venv/bin/activate
       pip install -r requirements.txt
       deactivate
       
       # Node.js dependencies
       npm install express nodemon jest supertest
       
       # Create environment files
       cat > .env.example << EOF
   NODE_ENV=development
   PORT=3000
   DATABASE_URL=postgresql://localhost:5432/$PROJECT_NAME
   SECRET_KEY=your-secret-key-here
   EOF
       
       cp .env.example .env
   }
   
   verify_setup() {
       log "✅ Verifying setup..."
       
       cd ~/$PROJECT_NAME
       
       # Test Node.js
       npm test 2>/dev/null || log "⚠️  Some tests failed - check setup"
       
       # Test Python
       source venv/bin/activate
       python3 -c "import flask; print('Python environment: OK')"
       deactivate
       
       # Test database connection
       psql -d $USER -c "SELECT version();" > /dev/null 2>&1 && log "Database: OK" || log "⚠️  Database connection issue"
       
       log "🎉 Development environment setup complete!"
       log "📂 Project location: ~/$PROJECT_NAME"
       log "🚀 To get started: cd ~/$PROJECT_NAME && source venv/bin/activate && npm run dev"
   }
   
   cleanup_partial_install() {
       log "🧹 Cleaning up partial installation..."
       
       # Remove incomplete project
       rm -rf ~/$PROJECT_NAME
       
       # Note: System packages are left installed as they might be useful
   }
   
   # Main setup process
   main() {
       log "🚀 Starting $PROJECT_NAME development environment setup"
       
       check_os
       install_system_dependencies
       install_nodejs
       install_python
       install_database
       setup_project_structure
       setup_environments
       verify_setup
       
       log "🎊 Setup completed successfully!"
   }
   
   # Run main setup
   main "$@"
   ```