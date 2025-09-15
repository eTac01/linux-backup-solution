# linux-backup-solution

# 🔐 Data Backup Scheme Implementation in Linux

## 📌 Overview
This project demonstrates the design and implementation of a **reliable and automated backup scheme** in Linux.  
It leverages:
- **Rsync** → for efficient file synchronization and backups  
- **Cron** → for scheduling periodic backups  
- **SSH** → for secure data transfer  

The solution ensures **data integrity, availability, and resilience** against accidental loss or corruption.  
It was developed as part of an academic project submitted to **L&T EduTech**.

---

## 🎯 Project Objectives
- Develop a **functional backup scheme** for Linux in a controlled environment.  
- Demonstrate **rsync-based backups** for directories/files.  
- Automate backups with **cron jobs**.  
- Test **reliability, error handling, and recovery**.  
- Document the **setup, findings, and recommendations**.  

---

## ⚙️ Methodology
1. **Environment Setup**
   - VirtualBox / VMware for test environment  
   - Linux (CentOS) as server and client  
   - Network-attached storage (NAS) for backup  

2. **Implementation**
   - Rsync for incremental backups  
   - Cron for scheduling daily/weekly/monthly jobs  
   - SSH for secure transfers  

3. **Testing**
   - Full, incremental, and partial restore  
   - Error handling (storage, permission, connectivity issues)  
   - Verification of data integrity and completeness  

---

#!/bin/bash
# ============================================================
# backup.sh - Automated Linux Backup Script
# ------------------------------------------------------------
# Author(s): Aabhash Paudel, Chudaraj Kushwaha, Satyam Raut
# Project: Data Backup Scheme Implementation in Linux
# Date: Feb 2024
# 
# Description:
#   This script uses rsync to backup important files/directories 
#   from a source location to a remote backup server over SSH.
#   It is designed for use with cron to automate daily/weekly 
#   backups.
# ============================================================

# -------- CONFIGURATION --------
# Source directory (what you want to back up)
SOURCE_DIR="/home/user/data"

# Destination (remote server + path)
DEST_USER="root"
DEST_IP="192.168.100.8"
DEST_DIR="/backupdestination/"

# Log file
LOG_FILE="/var/log/backup.log"

# Rsync options
# -a : archive mode (preserves permissions, symlinks, timestamps, etc.)
# -v : verbose output
# -z : compress file data during transfer
# --delete : delete files in destination that are no longer in source

RSYNC_OPTIONS="-avz --delete"

# -------- SCRIPT EXECUTION --------
echo "===============================" >> $LOG_FILE
echo "Backup started: $(date)" >> $LOG_FILE

# Run rsync command

rsync $RSYNC_OPTIONS -e ssh $SOURCE_DIR ${DEST_USER}@${DEST_IP}:$DEST_DIR >> $LOG_FILE 2>&1

# Check exit status
if [ $? -eq 0 ]; then

    echo "Backup completed successfully at $(date)" >> $LOG_FILE
else
    echo "Backup FAILED at $(date)" >> $LOG_FILE
fi

echo "===============================" >> $LOG_FILE


## 🖥️ Installation & Usage

### 1. Clone this repository:
```bash
   git clone https://github.com/<your-username>/data-backup-linux.git
   cd data-backup-linux

2. Configure backup.sh:

   Edit the script with your source, destination, and server IP.

3. Run a Manual Backup:
      bash src/backup.sh

4. Schedule with Cron:

   Open crontab:

      crontab -e


      Example (daily at 1 AM):

      0 1 * * * bash /path/to/backup.sh

📊 Findings

   -> Rsync transfers completed within allocated time.

   -> Backups remained unaffected even after deletion from the source.

   -> Scheduling worked smoothly using cron jobs.

   -> Scalability was validated using multiple Linux systems.

⚠️ Security Considerations

   -> Potential exploitation risks:

   -> Unauthorized access

   -> MITM attacks during transfer

   -> Injection of malicious code into backups

✅ Recommendations

   -> Use AES-256 encryption for backup data.

   -> Implement role-based access control & MFA.

   -> Regularly patch backup systems.

   -> Segment networks with firewalls & IDS/IPS.

   -> Conduct regular vulnerability assessments.


