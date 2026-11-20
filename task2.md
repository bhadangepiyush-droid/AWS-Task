Task 2
1. In the home directory, create → dir1/dir2/dir3/dir4
2. Inside dir2 , create 3 text files: file1.txt , file2.txt , file3.txt
3. Create a folder test in home
4. Copy file1.txt and file2.txt into test
5. Rename file3.txt to final.txt

# Task 2: Linux Basic Commands on AWS EC2

Second exercise from the **"Basic commands task"** PDF, performed on an **Amazon Linux 2023** EC2 instance over SSH from Git Bash (MINGW64) on Windows. It covers creating directories and files, copying files, fixing a failed copy, and renaming a file.

## Task Statement (Task 2 from the PDF)

1. In the home directory, create `dir1`, `dir2`, `dir3`, `dir4`
2. Inside `dir2`, create 3 text files: `file1.txt`, `file2.txt`, `file3.txt`
3. Create a folder `test` in the home directory
4. Copy `file1.txt` and `file2.txt` into `test`
5. Rename `file3.txt` to `final.txt`

## Environment

| Item | Details |
|------|---------|
| Cloud provider | AWS EC2 |
| Operating system | Amazon Linux 2023 |
| Login user | `ec2-user` |
| Instance private IP | `172.31.25.240` |
| Public DNS | `ec2-13-220-118-200.compute-1.amazonaws.com` |
| Key pair | `demo.pem` |
| Local terminal | Git Bash (MINGW64) on Windows |

## Step 0: Connect to the Instance

The session was reopened after exiting a previous one (a mistyped `by` returned `command not found`, then `exit` closed the connection). Reconnect with:

```bash
cd Downloads
ssh -i "demo.pem" ec2-user@ec2-13-220-118-200.compute-1.amazonaws.com
```

The SSH post-quantum key exchange warning is informational only. The prompt `[ec2-user@ip-172-31-25-240 ~]$` confirms a successful login.

## Step 1: Create the Directories

```bash
mkdir dir1 dir2 dir3 dir4
ls                       # dir1  dir2  dir3  dir4
```

> The PDF writes this as `dir1/dir2/dir3/dir4`. It was done as four separate directories in the home folder, the same way `p1/p2/p3` was treated in Task 1. Use `mkdir -p dir1/dir2/dir3/dir4` instead if a nested structure is wanted.

## Step 2: Create Files Inside `dir2`

```bash
cd dir2
ls                       # empty
touch file1.txt file2.txt file3.txt
ls                       # file1.txt  file2.txt  file3.txt
```

## Step 3: Create the `test` Folder

```bash
cd ..
mkdir test
ls                       # dir1  dir2  dir3  dir4  test
```

## Step 4: Copy Files into `test`

First attempt, run from the home directory:

```bash
cp dir2/file1.txt file2.txt test/
# cp: cannot stat 'file2.txt': No such file or directory
```

**Why it failed:** only the first source path included `dir2/`. `cp` looked for `file2.txt` in the current (home) directory, where it does not exist. `file1.txt` was copied correctly, but `file2.txt` was not.

**Fix:** give each file its full path.

```bash
cp dir2/file1.txt test/
cp dir2/file2.txt test/
cd test
ls                       # file1.txt  file2.txt
```

A single command would also work: `cp dir2/file1.txt dir2/file2.txt test/`

## Step 5: Rename `file3.txt` to `final.txt`

```bash
cd ..
cd dir2
ls                       # file1.txt  file2.txt  file3.txt
mv file3.txt final.txt
ls                       # file1.txt  file2.txt  final.txt
```

Linux has no separate rename command for this case, so `mv` is used to rename a file within the same directory.

## Final Directory State

```
/home/ec2-user
├── dir1/
├── dir2/
│   ├── file1.txt
│   ├── file2.txt
│   └── final.txt      (renamed from file3.txt)
├── dir3/
├── dir4/
└── test/
    ├── file1.txt      (copied from dir2)
    └── file2.txt      (copied from dir2)
```

## Commands Used

| Command | Purpose |
|---------|---------|
| `ssh -i "key.pem" user@host` | Connect to the EC2 instance |
| `ls` | List directory contents |
| `cd <dir>` / `cd ..` | Change directory / go up one level |
| `mkdir <dirs>` | Create one or more directories |
| `touch <files>` | Create empty files |
| `cp <src> <dest>` | Copy files |
| `mv <old> <new>` | Rename (or move) a file |
| `exit` | Close the SSH session |

## Key Learnings

- Each source path in `cp` is resolved relative to the **current directory**, so every file needs its own correct path.
- Reading the error message (`cannot stat ... No such file or directory`) points directly at the wrong path.
- `mv` both moves and renames files.
- Multiple directories and files can be created with one `mkdir` or `touch` command.

---
## Author

**Piyush Bhadange**
BE in Electronics and Telecommunication Engineering, cloud computing intern
GitHub: [bhadangepiyush-droid](https://github.com/bhadangepiyush-droid)