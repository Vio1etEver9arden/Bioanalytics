## 常用shell指令/Common shell commands

----
### 基本指令/Basic commands
#### `ls`
- `ls` : 列出当前目录的文件。List files and directories in current dictionary.  
- `ls -a`: 显示隐藏文件。Show hidden files.
    其中`.` 和`..`分别表示 当前目录 和 上一级目录，`.*` 代表隐藏文件。 
- `ls -l`: Show detailed files. 显示详细文件信息。 

#### `cd`
- `cd`: change directory. 更改目录。
- `pwd`: Show current directory. 显示当前目录。
- `cd ..`: go back to previous directory.返回上一级目录。
- `cd ~`: go to the home directory. 进入主目录。
- `cd "My Folder"`: Enter a directory with spaces. 进入包含空格的目录，加双引号。

#### `mkdir`
- `mkdir`: Make a directory. 创建文件夹。
- `mkdir -p project/code`: Make a nested directories. 创建多级目录。

#### `rm`
- `rm`: remove files. 删除文件。
- `rm -r`: remove directories. 删除文件夹。
- `rm -rf`: force remove files and directories. 强制删除文件和文件夹。


-----

### 环境/Environment
#### 新建环境/create environment
```bash
conda create -n rnaseq_env python=3.10
```
- `-n` : name of the environment. 环境名称。
- `python=3.10` : version of python. python版本。
- 这里新建个用于RNA-seq分析的环境，命名为`rnaseq_env`。
#### 激活环境/activate environment
```bash
conda activate rnaseq_env
```
#### 退出环境/deactivate environment
```bash
conda deactivate
```
#### 查看环境/list environments
```bash
conda env list
```
#### 保存/Save
```bash
conda env export > rnaseq_env.yml
```
- 如果想把环境导出到指定的文件夹，可以在文件名前加上路径。
- If you want to export the environment to a specified folder, you can add the path before the file name.

#### 删除环境/Delete environment
```bash
conda env remove -n rnaseq_env
```
- `-n` : name of the environment. 环境名称。
- `-y` : yes. 确认删除。


