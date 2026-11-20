Task 1
1. Create a folder structure in the home directory → p1/p2/p3
2. Inside p3 , create 5 empty .log files
3. Create two directories in home → backup1 and backup2
4. Copy all .log files from p3 to backup1
5. Move all files from backup1 to backup2

# Task 1: Linux File & Directory Management on AWS EC2

Hands-on task performed on an **Amazon Linux 2023** EC2 instance, connected from a local Git Bash (MINGW64) terminal over SSH. It covers creating, listing, removing, copying and moving directories and files using basic Linux commands.

## Environment

| Item | Details |
|------|---------|
| Cloud provider | AWS (Region: US East, N. Virginia) |
| Service | Amazon EC2 |
| Operating system | Amazon Linux 2023 |
| Login user | `ec2-user` |
| Security group | `sg-0393ebc1cfd00ab1f (test)` |
| Local terminal | Git Bash (MINGW64) on Windows |
| Key pair | `demo.pem` |

## Step 1: Connect to the EC2 Instance via SSH

Open a terminal in the folder containing the key pair and connect using the instance's Public DNS (command copied from the EC2 **Connect** page):

```bash
cd Downloads
ssh -i "demo.pem" ec2-user@ec2-44-222-205-74.compute-1.amazonaws.com
```

- On the first connection SSH shows a host authenticity prompt. Type `yes` to add the host to the known hosts list.
- A post-quantum key exchange warning appears. It is informational only and the login succeeds.
- The Amazon Linux 2023 banner and the prompt `[ec2-user@ip-172-31-24-199 ~]$` confirm a successful login.

## Step 2: Create Directories

```bash
ls                 # home directory is empty
mkdir p1 p2 p3     # create three directories at once
ls                 # p1  p2  p3
```

## Step 3: Create Empty Files

Move into `p3` and create five empty log files with `touch`:

```bash
cd p3
ls                 # empty
touch empty1.log empty2.log empty3.log empty4.log emtpy5.log
ls                 # empty1.log empty2.log empty3.log empty4.log emtpy5.log
```

> Note: the fifth file was typed as `emtpy5.log` (typo for `empty5.log`). It is kept as-is throughout the task.

## Step 4: Create and Remove a Directory

Go back to the home directory, create two backup directories (one with a misspelled name), then remove the wrong one:

```bash
cd ..
mkdir bacdup1 backup2
ls                 # bacdup1  backup2  p1  p2  p3
sudo rmdir bacdup1 # remove the misspelled directory
ls                 # backup2  p1  p2  p3
mkdir backup1      # create the correctly named directory
ls                 # backup1  backup2  p1  p2  p3
```

`rmdir` only removes **empty** directories, which is why it worked here.

## Step 5: Copy Files

Copy all `.log` files from `p3` into `backup1`:

```bash
cp p3/*.log backup1/
cd backup1
ls                 # empty1.log empty2.log empty3.log empty4.log emtpy5.log
```

`cp` leaves the originals in `p3`, so the files now exist in both places.

## Step 6: Move Files

Move all `.log` files from `backup1` into `backup2`:

```bash
cd ..
mv backup1/*.log backup2
cd backup2
ls                 # empty1.log empty2.log empty3.log empty4.log emtpy5.log
```

`mv` relocates the files, so `backup1` is now empty and `backup2` holds the log files.

## Final Directory State

```
/home/ec2-user
├── backup1/        (empty, files were moved out)
├── backup2/
│   ├── empty1.log
│   ├── empty2.log
│   ├── empty3.log
│   ├── empty4.log
│   └── emtpy5.log
├── p1/
├── p2/
└── p3/
    ├── empty1.log
    ├── empty2.log
    ├── empty3.log
    ├── empty4.log
    └── emtpy5.log
```

## Commands Used

| Command | Purpose |
|---------|---------|
| `ssh -i "key.pem" user@host` | Connect to the EC2 instance securely |
| `ls` | List directory contents |
| `cd <dir>` / `cd ..` | Change into a directory / go up one level |
| `mkdir <dirs>` | Create one or more directories |
| `rmdir <dir>` | Remove an empty directory |
| `touch <files>` | Create empty files |
| `cp <src> <dest>` | Copy files |
| `mv <src> <dest>` | Move (or rename) files |
| `sudo` | Run a command with administrator privileges |

## Key Learnings

- Connecting securely to an EC2 instance with a `.pem` key pair over SSH.
- Creating multiple directories and files with a single command.
- Using wildcards (`*.log`) to act on many files at once.
- The difference between `cp` (duplicate) and `mv` (relocate).
- `rmdir` removes only empty directories.

## Screenshots

The task was documented with six terminal screenshots, in order:

1. SSH login and the empty home directory
2. Creating `p1`, `p2`, `p3`
3. Entering `p3`
4. Creating the five `.log` files
5. Creating, removing and copying into backup directories
6. Moving files from `backup1` to `backup2`

---
## Author

**Piyush Bhadange**
BE in Electronics and Telecommunication Engineering, cloud computing intern
GitHub: [bhadangepiyush-droid](https://github.com/bhadangepiyush-droid)