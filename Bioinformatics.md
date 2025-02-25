# Bioinformatics 

### From Du

__Update Time__ : 2025-02-21       
> (It's still under construction)

__(Configure the environment构建环境)__   

This is the tutorial I followed构建linux环境 : [Bilibili](https://www.bilibili.com/video/BV12x411Z7Es/?spm_id_from=333.1391.0.0&vd_source=9013f14436bf85913f2eaa9bb7094f21)

这是Markdown的教程：[Markdown tutorial](https://markdown.com.cn/basic-syntax/)

---
## Chip-seq

- ###  使用BedgraphToBigwig 将bdg转换成bigwig文件
     > Convert .bdg to .bigwig  

1. __Navigate to the Target Directory__
   

   Given all files are in Process folder, 在终端运行
   ```bash
   cd /Users/yilindu/Documents/chip-seq202502/Process
   ```

2. __Ensure `chrom.size` file exits. 确保基因组文件存在__


   ```bash
   ls -l hg38.chrom.sizes
   ```
   if it doesn't exits, download. take `hg38` as an example.  你也可以选择`hg19`, `mm10`, etc.
   
    
   ```bash
   curl -O http://hgdownload.cse.ucsc.edu/goldenpath/hg38/bigZips/hg38.chrom.sizes
   ```

3. __Convert__  
   ```bash
   bedGraphToBigWig sorted_input.bedgraph hg38.chrom.sizes output.bw
   ```
   其中，`sorted_input.bedgraph`是bdg文件名，`hg38.chrom.sizes` is the name of reference genome, `output.bw` is the bigwig name of output

- ### 合并bigwig文件 Merge bigwig files
  使用 `bigWigMerge` 合并
  ```bash
  bigWigMerge GSM2527635.bigwig GSM2527636.bigwig GSE96003.bedGraph
  ```
  这样子是先合并成bedgraph文件，然后再转换成bigwig文件


