No problem! Here is the documentation translated into English:

---

## PostgreSQL Database Backup Automation

This repository contains scripts to automate the backup of PostgreSQL databases and upload the backups to an S3 bucket, with optional support for scheduling via cron and encryption. Below are the instructions for configuring and using the backup process.

### Files

- **`backup.sh`**: Main script that performs the PostgreSQL database backup and uploads it to the S3 bucket.
- **`install.sh`**: Installation script that sets up the necessary dependencies.
- **`run.sh`**: Execution script that runs the backup immediately or schedules it based on the configuration.

### Configuration

The scripts use environment variables to configure access credentials for S3, PostgreSQL database information, and other parameters. Make sure to correctly set the following variables:

- **S3_ACCESS_KEY_ID**: Access key for the S3 bucket.
- **S3_SECRET_ACCESS_KEY**: Secret key for the S3 bucket.
- **S3_BUCKET**: Name of the S3 bucket where the backup will be stored.
- **S3_REGION**: Region of the S3 bucket.
- **S3_ENDPOINT** (optional): Custom endpoint for S3 if you're not using AWS.
- **POSTGRES_HOST**: Address of the PostgreSQL server.
- **POSTGRES_PORT**: Port of the PostgreSQL server.
- **POSTGRES_USER**: PostgreSQL database user.
- **POSTGRES_PASSWORD**: Password for the PostgreSQL user.
- **POSTGRES_DATABASE**: Name of the database to be backed up. Leave empty and use `POSTGRES_BACKUP_ALL=true` to back up all databases.
- **SCHEDULE**: Backup schedule. Use cron format (e.g., `0 2 * * *` to run at 2 a.m.). If not set, the backup will run immediately.
- **ENCRYPTION_PASSWORD** (optional): Password to encrypt the backup using AES-256.

### Usage

1. **Install dependencies**  
   Run the `install.sh` script to install the required PostgreSQL client, `aws-cli`, and `openssl` needed for the backup process.

   ```bash
   sh install.sh
   ```

2. **Run the backup**  
   You can run the backup immediately or schedule it using cron. For an immediate backup, run the `run.sh` script without setting the schedule variable.

   ```bash
   sh run.sh
   ```

   To schedule the backup, set the `SCHEDULE` environment variable with the desired cron expression.

   ```bash
   export SCHEDULE="0 2 * * *"
   sh run.sh
   ```

3. **Set environment variables**  
   Ensure the required environment variables are set before running the scripts. Here's an example:

   ```bash
   export S3_ACCESS_KEY_ID=your-access-key-id
   export S3_SECRET_ACCESS_KEY=your-secret-access-key
   export S3_BUCKET=your-s3-bucket
   export POSTGRES_HOST=your-postgres-host
   export POSTGRES_USER=your-postgres-user
   export POSTGRES_PASSWORD=your-postgres-password
   export POSTGRES_DATABASE=your-database
   ```

### Encryption

If you want to encrypt the backups before uploading them to S3, set the `ENCRYPTION_PASSWORD` environment variable. The backup will be encrypted using AES-256.

```bash
export ENCRYPTION_PASSWORD=your-encryption-password
```

### Scheduling with cron

To schedule automatic backups, set the `SCHEDULE` variable with the appropriate cron expression. The `run.sh` script will configure the schedule and use `go-crond` to manage the cron job.

### Dockerfile Example

You can create a Docker container to run these backup scripts. Here’s an example Dockerfile:

```dockerfile
FROM alpine:3.18

COPY backup.sh /backup.sh
COPY install.sh /install.sh
COPY run.sh /run.sh

RUN sh /install.sh

CMD ["sh", "/run.sh"]
```

### Logs

Execution logs, including any errors, will be displayed in the console. If the backup fails, the process will abort, and the corresponding error will be shown.

### Contribution

Feel free to contribute with improvements or adjustments. You can create a pull request or open an issue with suggestions.
