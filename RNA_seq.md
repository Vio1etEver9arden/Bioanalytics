# RNA-seq  

-------
## 使用sra-tools下载数据 Download data using sra-tools 
> 使用`sra-tools`工具。`sra-tools`中包含了`prefetch`和`fasterq-dump`。官方推荐的做法是先通过`prefetch`下载.sra数据，然后使用`fasterq-dump`转换成fastq格式。经我个人测试这是速度最快的。当然也可以直接使用`fasterq-dump`下载fastq格式的数据。
> We use `sra-tools` to download data. `sra-tools` includes `prefetch` and `fasterq-dump`. The official recommendation is to first download the `sra` format data using `prefetch`, and then convert it to `fastq` format using `fasterq-dump`. In my personal test, this is the fastest. Of course, you can also use `fasterq-dump` to download fastq format data directly.
1. 安装sra-tools/Install sra-tools
   Follow the [Documents](https://github.com/ncbi/sra-tools)
   ```
   conda install -c bioconda sra-tools
   ```
   Or download manually.
   <br>
2. 下载数据/Download data
  - Use `prefetch`
     > Use`prefetch -h` to check help and options.
     ```
     prefetch SRA1234567 -p -O SRA
     ```
     其中/Among the parameters:
     - `SRA1234567` is the SRR number,
     - `-p` is to show progress.
     - `-O SRA` is the output directory named `SRA`. It's an optional parameter. 
     - 输入也可以是SRR list文件/The input can also be a SRR list file: `--option-file SRR_Acc_List.txt`. 值得注意的是，`prefetch`会为每个SRR创建一个文件夹，文件夹名称为SRR号/The `prefetch` will create a folder for each SRR, and the folder name is the SRR number.
  - (Not recommended) Use `fasterq-dump` to download
     > Use`fasterq-dump -h` to check help and options.
     ```bash
     fasterq-dump SRR1234567 --split-files -e 10 -O ./fastq_output -p
     ```
     其中/Among the parameters:
    
     - `SRR1234567` is the SRR number
     - `--split-files` is to split paired-end reads. __ATTENTION ⚠️:  It depends on the seq data (single or paired end). Please see [official documents](https://github.com/ncbi/sra-tools/wiki/HowTo:-fasterq-dump).__
     - `-e 10` is the number of threads (according to your computer),
     - `-O ./fastq_output` is the output directory.
     - `-p` is to show progress.    
3. Convert to fastq/转换成fastq格式  
   ```bash
   fasterq-dump SRR1234567.sra --split-files -e 10 -O ./fastq_output -p
   ```
   - 其中只是将编号改成了.sra文件/Only the number is changed to .sra file.
4. 压缩fastq文件/Compress fastq files
   - `fasterq-dump` doesn't have `--gzip` option, so you need to compress the files manually. `pigz` is recommended. 不能使用`--gzip`选项，所以需要手动压缩文件。建议使用`pigz`
    ```bash
    pigz -p 10 *.fastq
    ```
5. 批量处理

   ```bash
   mkdir -p SRA fastq_output
   for acc in $(cat SRR_Acc_List.txt);
    do
      prefetch $acc \
       -p \
       -O SRA 

      fasterq-dump ./SRA/$acc/$acc.sra \  # Due to the prefetch, the sra file is in the folder named by SRR number.
      --split-files \
      -e 10 \
      -O ./fastq_output \
      -p
   done < SRR_Acc_List.txt 

   pigz -p 10 ./fastq_output/*.fastq
   ```
## FastQC - Quality control/质量控制
> FastQC is a tool using to check the quality of sequencing data before and after trimming.
> FastQC用于检查测序数据质量。
>  