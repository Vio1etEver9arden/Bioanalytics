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
     fasterq-dump SRR1234567 --split-files -e 16 -O ./fastq_output -p
     ```
     其中/Among the parameters:
    
     - `SRR1234567` is the SRR number
     - `--split-files` is to split paired-end reads. __ATTENTION ⚠️:  It depends on the seq data (single or paired end). Please see [official documents](https://github.com/ncbi/sra-tools/wiki/HowTo:-fasterq-dump).__
     - `-e 16` is the number of threads (according to your computer),
     - `-O ./fastq_output` is the output directory.
     - `-p` is to show progress.    
3. Convert to fastq/转换成fastq格式  
   ```bash
   fasterq-dump SRR1234567.sra --split-files -e 16 -O ./fastq_output -p
   ```
   - 其中只是将编号改成了.sra文件/Only the number is changed to .sra file.
4. 压缩fastq文件/Compress fastq files
   - `fasterq-dump` doesn't have `--gzip` option, so you need to compress the files manually. `pigz` is recommended. 不能使用`--gzip`选项，所以需要手动压缩文件。建议使用`pigz`
    ```bash
    pigz -p 16 *.fastq
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
> FastQC is a tool used to check the quality of sequencing data before and after trimming.
> FastQC用于检查测序数据质量。

> `fastqc -h` to check help and options.

> See [official website](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/) for more details.
1. 安装/Install
   ```bash
   conda install -c bioconda fastqc
   ```
2. 使用/Usage
   ```bash
   mkdir -p fastqc  # Create a directory for fastqc output

   fastqc ./fastq_output/*.fastq.gz -o ./fastqc --extract -t 16
    ```
    其中/Among the parameters:
    - `./fastq_output/*.fastq.gz` is the input fastq files,
    - `-o ./fastqc` is the output directory,
    - `--extract` is to extract the zip file.
    - `-t 16` is the number of threads (according to your computer).
3. 查看结果/Check results
   The output `html` files are in the "fastqc" output directory and can be opened in a web browser.
4. 结果分析/Result analysis
5. (可选/Optional) 使用`multiqc`合并结果/Use `multiqc` to merge results
   > `multiqc -h ` Check help and options

   > See [official website](https://docs.seqera.io/multiqc) for more details. 
   ```bash
   # Install multiqc
   conda install -c bioconda multiqc
   ```
   ```bash
   multiqc ./fastqc -o ./multiqc
   ```
   其中/Among the parameters:
    - `./fastqc` is the input directory. MultiQC will scan the specified directory (. is the current dir) and produce a report detailing whatever it finds.
    - `-o ./multiqc` is the output directory.

## Trimming/修剪
> Depending on the FastQC results, you may need to trim the reads.
> There are many tools for trimming, such as `trimmomatic`, `cutadapt`, `fastp`, etc. We will use `Trim Galore!` here.

> `Trim Galore!` is a wrapper around __`cutadapt`__ and __`fastqc`__, so you have to install them first.

> `trim_galore --help` to check help and options.

> See [Offical user guide](https://github.com/FelixKrueger/TrimGalore/blob/master/Docs/Trim_Galore_User_Guide.md) for more details.

1. 安装/Install
   ```bash
   conda install -c bioconda trim-galore
   ```
2. 使用/Usage
   - paired end reads
     ```bash
     trim_galore --paired \
     --fastqc \
     --o trimmed \
     -cores 4  \

     ./fastq_output/SRR28810090_1.fastq.gz ./fastq_output/SRR28810090_2.fastq.gz
     ```
      其中/Among the parameters:
      - `--paired` is for paired-end reads,
      - `--fastqc` is to run FastQC once trimming finished,
      - `--o` is the output directory for trimmed files,
      - `-cores` specifies the number of threads to use. 4 threads are recommended in the official documents. 
       
     __ATTENTION ⚠️:__ Many parameters are defaulted and you can customize in options. 
3. 批量处理/Batch processing
   ```bash
   for r1 in ./fastq_output/*_1.fastq.gz; do
    base=$(basename "$r1" _1.fastq.gz)
    r2="./fastq_output/${base}_2.fastq.gz"

    trim_galore --paired \
     --o trimmed \
     --fastqc \
     --cores 4 \
     "$r1" "$r2"
    done
    ```


   