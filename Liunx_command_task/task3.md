Task 3
1. Create a folder projectX in home
2. Inside projectX , create subfolders → src , bin , docs
3. Inside src , create 4 files: a.txt , b.txt , c.txt , d.txt
4. Copy all .txt files to docs
5. Move d.txt from docs to bin

# Task 3: Linux Basic Commands on AWS EC2

Third exercise from the **"Basic commands task"** PDF, performed on an **Amazon Linux 2023** EC2 instance over SSH from Git Bash (MINGW64) on Windows. It covers creating a project folder structure, creating files, copying files with a wildcard, fixing two failed commands, and moving a file.

## Task Statement (Task 3 from the PDF)

1. Create a folder `projectX` in the home directory
2. Inside `projectX`, create subfolders `src`, `bin`, `docs`
3. Inside `src`, create 4 files: `a.txt`, `b.txt`, `c.txt`, `d.txt`
4. Copy all `.txt` files to `docs`
5. Move `d.txt` from `docs` to `bin`

## Environment

| Item | Details |
|------|---------|
| Cloud provider | AWS EC2 |
| Operating system | Amazon Linux 2023 |
| Login user | `ec2-user` |
| Instance private IP | `172.31.18.203` |
| Public DNS | `ec2-3-85-37-74.compute-1.amazonaws.com` |
| Key pair | `demo.pem` |
| Local terminal | Git Bash (MINGW64) on Windows |

## Step 0: Connect to the Instance

The first attempt failed with `bash: $'\E[200~ssh': command not found`. A hidden paste character (`^[[200~`) was added in front of the command. Retyping it worked:

```bash
cd Downloads
ssh -i "demo.pem" ec2-user@ec2-3-85-37-74.compute-1.amazonaws.com
```

On first connection SSH asks to trust the host, and answering `yes` adds it to `known_hosts`. The post-quantum key exchange warning is informational only. The prompt `[ec2-user@ip-172-31-18-203 ~]$` confirms a successful login.

## Step 1: Create `projectX`

```bash
ls                       # empty
mkdir projectX
ls                       # projectX
```

## Step 2: Create the Subfolders

```bash
cd projectX/
mkdir src bin docs
ls                       # bin  docs  src
```

## Step 3: Create Files Inside `src`

```bash
cd src
touch a.txt b.txt c.txt d.txt
ls                       # a.txt  b.txt  c.txt  d.txt
```

## Step 4: Copy All `.txt` Files to `docs`

First attempt, run from the home directory:

```bash
cd ..
cd ..
cp src/*.txt docs
# cp: cannot stat 'src/*.txt': No such file or directory
```

**Why it failed:** after two `cd ..` commands the shell was in the home directory, where `src` does not exist. The path `src/*.txt` is relative to the current directory.

**Fix:** go back into `projectX` and run it again.

```bash
cd projectX
cp src/*.txt docs        # no output means success
ls                       # bin  docs  src
```

## Step 5: Move `d.txt` to `bin`

First attempt, with a typo:

```bash
cd ..
mv scr/d.txt bin
# mv: cannot stat 'scr/d.txt': No such file or directory
```

**Why it failed:** `scr` was typed instead of `src`.

**Fix:** correct the path.

```bash
mv src/d.txt bin
cd bin
ls                       # d.txt
```

> **Note:** the task says to move `d.txt` from `docs`, but the screenshots show it moved from `src`. The copy of `d.txt` made in Step 4 is therefore still in `docs`, and `src` no longer has `d.txt`. To follow the task exactly, run `mv docs/d.txt bin` from inside `projectX`.

## Final Directory State (as performed in the screenshots)

```
/home/ec2-user
└── projectX/
    ├── bin/
    │   └── d.txt          (moved from src)
    ├── docs/
    │   ├── a.txt          (copied from src)
    │   ├── b.txt
    │   ├── c.txt
    │   └── d.txt
    └── src/
        ├── a.txt
        ├── b.txt
        └── c.txt
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
| `mv <old> <new>` | Move (or rename) a file |

## Key Learnings

- Relative paths depend on the **current directory**. Check where you are before running `cp` or `mv`.
- Read the error message (`cannot stat ... No such file or directory`). It points directly at the wrong path.
- A small typo (`scr` vs `src`) is enough to break a command. Tab completion helps avoid this.
- The wildcard `*.txt` matches every file ending in `.txt`.
- Several directories or files can be created with one `mkdir` or `touch` command.
- Pasting into the terminal can add hidden characters, so retype the command if you see an odd error.

---
## Author

**Piyush Bhadange**
BE in Electronics and Telecommunication Engineering, cloud computing intern
GitHub: [bhadangepiyush-droid](https://github.com/bhadangepiyush-droid)
