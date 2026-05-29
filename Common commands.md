## Common commands

---

### Basic commands

#### `ls`

- `ls`: List files and directories in the current directory.
- `ls -a`: Show hidden files. `.` and `..` represent the current and parent directories respectively; `.*` matches hidden files.
- `ls -l`: Show detailed file information.
  - `-a`: include entries starting with `.` (hidden files).
  - `-l`: use long listing format (permissions, owner, size, date).

#### `cd`

- `cd`: Change directory.
- `pwd`: Show current directory.
- `cd ..`: Go up one directory.
- `cd ~`: Go to the home directory.
- `cd "My Folder"`: Enter a directory with spaces by wrapping the name in quotes.

#### `mkdir`

- `mkdir`: Create a directory.
- `mkdir -p project/code`: Create nested directories.
  - `-p`: create parent directories as needed (no error if existing).

#### `rm`

- `rm`: Remove files.
- `rm -r`: Remove directories recursively.
- `rm -rf`: Force remove files and directories.
  - `-r` / `-R`: recursive (remove directories and contents).
  - `-f`: force (ignore nonexistent files, never prompt).

---

### Environment

#### 1. Create environment

```bash
conda create -n rnaseq_env python=3.10
```

- `-n`: Environment name.
- `python=3.10`: Python version.
- This creates an environment for RNA-seq analysis named `rnaseq_env`.

#### 2. Activate environment

```bash
conda activate rnaseq_env
```

#### 3. Deactivate environment

```bash
conda deactivate
```

#### 4. List environments

```bash
conda env list
```

#### 5. Export environment

```bash
conda env export > rnaseq_env.yml
```

- To export to a specific folder, include the path before the file name.

#### 6. Remove environment

```bash
conda env remove -n rnaseq_env
```

- `-n`: Environment name.
- `-y`: Confirm removal.

---
