## Preparation

**Introduction to CITE-Seq:**
[:fontawesome-solid-file-pdf: Download the presentation](scripts/Introduction_to_CITESeq.pdf){: .md-button }

Download the required files (found on the shared folder) to your computer in a project/course directory. 

## Exercise
We already ran cellranger multi to save some time

```sh
cellranger multi --id='SAMPLE' --csv='ProjectDir'/configs/'SAMPLE'.csv
```
The config file for each sample looks like this:

```sh
[gene-expression]
reference,/path_to_reference/refdata-cellranger-GRCh38-3.0.0
expect-cells,10000

[feature]
reference,/path_to_feature_file/feature_ref.csv

[libraries]
fastq_id,fastqs,lanes,feature_types,subsample_rate
'SAMPLE'_SP,/path_to_raw_data,,Antibody Capture,
'SAMPLE'_GX,/path_to_raw_data,,Gene Expression,

```
feature_ref.csv file is set up in this manner:

```sh
id, name,read,pattern,sequence,feature_type
CD69,CD69,R2,5PNNNNNNNNNN(BC)NNNNNNNNN,GTCTCTTGGCTTAAA,Antibody Capture
CD8,CD8,R2,5PNNNNNNNNNN(BC)NNNNNNNNN,GCGCAACTTGATGAT,Antibody Capture
CD4,CD4,R2,5PNNNNNNNNNN(BC)NNNNNNNNN,TGTTCCCGCTCAACT,Antibody Capture
CD103,CD103,R2,5PNNNNNNNNNN(BC)NNNNNNNNN,GACCTCATTGTGAAT,Antibody Capture
```


Install the required packages "Seurat" and "ggplot2"

??? Tip "Answer"
    ```sh
    library(Seurat)
    library(ggplot2)
    ```


## Let's get started

Set an project directory as working directory and a create a folder for the results

??? Tip "Answer"
    ```sh
    project_dir <- "~/Documents/immunology-in-bioinformatics-training/hands_on/"
    setwd(project_dir)
    out_folder <- paste0(project_dir,"results/")
    dir.create(out_folder)
    ```


Run 'Read10X' in the folder containing the cellranger output

??? Tip "Answer"
    ```sh
    i.data <- Read10X("filtered_feature_bc_matrix/")
    ```


Now we have a list with two elements. We split this list now into two elements. 
First we will create a seurat object containing the gene expression date using 'CreateSeuratObject'. 
And second we will turn the CITE-seq data into a matrix using 'as.matrix'. 
Please save the resulting objects as '.rds' files into the results directory.


??? Tip "Answer"

    ```sh
    sc_data <- CreateSeuratObject(counts=i.data$`Gene Expression`, min.cells=3, min.features=200)
    saveRDS(sc_data, file=paste0(out_folder, "seurat_gene_expression.rds"))

    CITEseq_data <- as.matrix(i.data$`Antibody Capture`)
    saveRDS(CITEseq_data, file=paste0(out_folder, "antibody_capture_matrix.rds"))
    ```


We can look at the CITE-seq data with summary(t(CITEseq_data)). To get a normal distribution we will 
take the log of the data. 

??? Tip "Answer"

    ```sh
    data_log <- as.data.frame(apply(t(CITEseq_data),c(1,2),log))
    ```


We can plot the log-data using ggplot. Let's start with CD4 (The antibody is calles 'CD4.1') vs CD8.

??? Tip "Answer"

    ```sh
    gg <- ggplot(data_log, aes(x = CD4.1, y = CD8)) +
    geom_bin2d(bins=200) + xlab("CD4") + ylab("CD8") +
    ggtitle("CD4 vs. CD8 ")
    print(gg)
    ```


There is a separation of CD4+ and CD4- cells at around 3.2. CD8 is a bit more complicated. 
CD4+ cells look like they bound some CD8 antibody. Le't add some lines to the plot to split the cell populations.

??? Tip "Answer"

    ```sh
    gg + 
    geom_vline(xintercept=3.2, linetype="dashed", color = "red") +
    geom_abline(intercept = 0, slope = 1, linetype="dashed", color = "red") +
    geom_hline(yintercept=3.2, linetype="dashed", color = "red")

    ```

We can now split our cell into CD4 cells, CD8 cells, double-neg (DN) and douple-pos (DP) cells


??? Tip "Answer"

    ```sh
    CD4 <- data_log$CD4.1 > 3.2 & data_log$CD8 <= data_log$CD4.1
    CD8 <- data_log$CD4.1 <= 3.2 & data_log$CD8 > 3.2
    DP <- data_log$CD4.1 > 3.2 & data_log$CD8 > data_log$CD4.1
    DN <- data_log$CD4.1 <= 3.2 & data_log$CD8 <= 3.2
    ```

Let's do the same with CD69 (Antibody is called CD69.1) and CD103.


??? Tip "Answer"

    ```sh
    gg <- ggplot(data_log[CD4,], aes(x = CD69.1, y = CD103)) +
    geom_bin2d(bins=200) + xlab("CD69.1") + ylab("CD103") +
    geom_vline(xintercept=2.6, linetype="dashed", color = "red") +
    geom_hline(yintercept=3, linetype="dashed", color = "red") +
    ggtitle("CD69 vs. CD103 ")
    print(gg)


    CD69_CD103 <- data_log$CD69.1 > 2.6 & data_log$CD103 > 3
    CD69 <- data_log$CD69.1 > 2.6 & data_log$CD103 <= 3
    CD103 <- data_log$CD69.1 <= 2.6 & data_log$CD103 > 3
    DN2 <- data_log$CD69.1 <= 2.6 & data_log$CD103 <= 3
    ```

We are now ready to create a lable for our cells. They all belong in two categories. 
The first can be CD4/CD8/DN/DP and second CD69/CD103/DN/DP

??? Tip "Answer"

    ```sh
    CITE_label <- rep(NA,times= nrow(data_log))
    CITE_label[CD4] <- "CD4"
    CITE_label[CD8] <- "CD8"
    CITE_label[DP] <- "CD4_CD8"
    CITE_label[DN] <- "DN"

    CITE_label[CD69_CD103] <- paste0(CITE_label[CD69_CD103],"_CD69_CD103")
    CITE_label[CD69] <- paste0(CITE_label[CD69],"_CD69")
    CITE_label[CD103] <- paste0(CITE_label[CD103],"_CD103")
    CITE_label[DN2] <- paste0(CITE_label[DN2],"_DN")
    ```

We can look at the results with table. Furthermore we can add this labels to the "raw" 
CITE_Abs counts to add everything as metadata to our seurat object containing the expression data.

??? Tip "Answer"

    ```sh
    table(CITE_label)

    final_CITE <- cbind(t(CITEseq_data),CITE_label)

    seu <- AddMetaData(sc_data, final_CITE)

    ```

If you are done with the Hands-on part. Here are a few more excersises you can do while you wait for the others to finish.

1.  Compare the gene expression of CD4 to the CITE-Seq results for CD4

2.  Subset your seurat object so it only contains CD4+ T cells. And subcuster these cells using the expression data









