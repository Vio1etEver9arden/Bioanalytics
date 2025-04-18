## RNA-seq
### 使用fasterq-dump下载数据 Download data using fasterq-dump
使用sra-tools工具
1. 安装sra-tools/install sra-tools
   [Documents](https://github.com/ncbi/sra-tools)
   ```bash
   conda install -c bioconda sra-tools
   ```
   或者本地下载安装/or download and install locally  
   <br>

  2. 下载数据/download data
   ```bash
   fasterq-dump SRR1234567 --split-files -e 10 -O ./fastq_output -p
   ```
 其中/Among the parameters:
    
  - `SRR1234567`is the SRR number,
  - `--split-files` is to split paired-end reads,
  - `-e 10` is the number of threads (according to your computer),
  - `-O ./fastq_output` is the output directory.
  - `-p` is to show progress.  
  
  fasterq-dump doesn't have `--gzip` option, so you need to compress the files manually. 不能使用`--gzip`选项，所以需要手动压缩文件。建议使用`pigz`
  ```bash
  pigz -p 10 *.fastq
  ```