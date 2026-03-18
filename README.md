# Log-Archive-Tool
Log archival tool that compresses logs with timestamp and maintains archive records. Archive logs from any directory, schedule with cron, keep system clean.


# log-archive

🗂️ CLI tool to compress and archive logs with automatic timestamping. Keep your system clean while maintaining compressed logs for future reference.

## Features

- ✅ **Easy to use** - Single command with directory argument
- ✅ **Automatic timestamps** - Files named with `logs_archive_YYYYMMDD_HHMMSS.tar.gz`
- ✅ **Compressed storage** - Uses tar.gz for efficient space usage
- ✅ **Archive logging** - Maintains record of all archives with dates
- ✅ **Error handling** - Validates directories and handles failures gracefully
- ✅ **Colored output** - Clear visual feedback during operation
- ✅ **Schedulable** - Works with cron for automated archival

## Requirements

- Linux/Unix system
- `bash` shell
- `tar` command
- `date` command
- Standard GNU utilities

## Installation

### System-wide Installation (Recommended)

```bash
chmod +x log-archive
sudo cp log-archive /usr/local/bin/
```

Now run from anywhere:
```bash
log-archive /var/log
```

### Local Installation

```bash
chmod +x log-archive
./log-archive /var/log
```

## Usage

### Basic Usage

```bash
log-archive <log-directory>
```

### Examples

Archive system logs:
```bash
log-archive /var/log
```

Archive application logs:
```bash
log-archive /var/log/nginx
log-archive /var/log/apache2
log-archive /app/logs
```

## Output

```
Starting archive...
Archiving: /var/log
Output: ./log_archives/logs_archive_20240816_100648.tar.gz
✓ Archive successful!
Archive: logs_archive_20240816_100648.tar.gz
Size: 2.3M
Location: ./log_archives/logs_archive_20240816_100648.tar.gz
```

## Directory Structure

```
log_archives/
├── logs_archive_20240816_100648.tar.gz
├── logs_archive_20240816_110000.tar.gz
├── logs_archive_20240816_120000.tar.gz
└── archive.log
```

## Archive Log File

The tool maintains `archive.log` with all archive records:

```
[2024-08-16 10:06:48] Archived: logs_archive_20240816_100648.tar.gz | Size: 2.3M | Source: /var/log
[2024-08-16 11:00:00] Archived: logs_archive_20240816_110000.tar.gz | Size: 1.8M | Source: /var/log
[2024-08-16 12:00:00] Archived: logs_archive_20240816_120000.tar.gz | Size: 2.1M | Source: /var/log
```

## Schedule with Cron

Archive logs daily at 2 AM:

```bash
crontab -e
```

Add this line:
```bash
0 2 * * * log-archive /var/log
```

Archive every 6 hours:
```bash
0 */6 * * * log-archive /var/log
```

Archive every day at midnight:
```bash
0 0 * * * log-archive /var/log
```

## Extract Archives

View contents without extracting:
```bash
tar -tzf log_archives/logs_archive_20240816_100648.tar.gz
```

Extract archive:
```bash
tar -xzf log_archives/logs_archive_20240816_100648.tar.gz
```

Extract to specific directory:
```bash
tar -xzf log_archives/logs_archive_20240816_100648.tar.gz -C /tmp/
```

## How It Works

1. **Accepts directory argument** - User provides path to log directory
2. **Validates directory** - Ensures directory exists
3. **Creates archive directory** - `./log_archives/` if it doesn't exist
4. **Generates timestamp** - Current date and time in `YYYYMMDD_HHMMSS` format
5. **Compresses logs** - Uses `tar -czf` to create gzip compressed archive
6. **Logs operation** - Records timestamp, filename, size, and source
7. **Displays results** - Shows success/failure with file details

## Error Handling

The script handles common errors:

```bash
# Missing argument
$ log-archive
Error: Log directory not provided
Usage: log-archive <log-directory>

# Non-existent directory
$ log-archive /nonexistent
Error: Directory '/nonexistent' does not exist

# Permission denied
$ log-archive /root/private
(Fails gracefully with error in archive.log)
```

## Tips & Best Practices

### 1. Regular Cleanup
Remove old archives to save space:
```bash
find log_archives/ -name "*.tar.gz" -mtime +30 -delete
```

### 2. Monitor Archive Size
Check total archive size:
```bash
du -sh log_archives/
```

### 3. Backup Archives
Copy archives to remote storage:
```bash
tar -czf log_archives/logs_archive_*.tar.gz | ssh user@server 'cat > /backups/logs.tar.gz'
```

### 4. Verify Archives
Check archive integrity:
```bash
tar -tzf log_archives/logs_archive_*.tar.gz > /dev/null && echo "OK" || echo "FAILED"
```

## Advanced Usage

### Upload to S3
```bash
#!/bin/bash
log-archive /var/log
aws s3 cp log_archives/*.tar.gz s3://my-bucket/logs/
```

### Email Notification
```bash
#!/bin/bash
log-archive /var/log
echo "Archive complete" | mail -s "Log Archive Done" admin@example.com
```

### Delete Original Logs After Archive
```bash
log-archive /var/log
rm -rf /var/log/*.log*
```

## Troubleshooting

### Permission Denied
```bash
chmod +x log-archive
```

### Archives not created in expected location
The script creates `log_archives/` in the current working directory. Change directory first:
```bash
cd /data
log-archive /var/log
```

### Cron not running
Check cron logs:
```bash
grep CRON /var/log/syslog
```

Make sure script is in PATH:
```bash
which log-archive
```

## License

MIT License - Feel free to use and modify

## Contributing

Submit issues and pull requests for improvements!

## Author

Built as a practical DevOps tool for efficient log management.

---

**Keep your system logs organized and compressed!** 📦
