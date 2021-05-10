# Code: treemap

Test to plot a treemap w/ `R`

## Most basic treemap with R

* https://www.r-graph-gallery.com/234-a-very-basic-treemap.html
* customize labels & colors: https://www.r-graph-gallery.com/236-custom-your-treemap.html
* also possible in Python:
    * https://towardsdatascience.com/treemap-basics-with-python-777e5ed173d0

```bash
conda create -n treemap -c conda-forge r-treemap
```

```R
# library
library(treemap)
 
# Create data
lineage <- c("B.1.1.7","B.1.351","A.27")
count <- c(27,5,9)
data <- data.frame(lineage,count)
 
# treemap
pdf('treemap.pdf')
treemap(data,
            index="lineage",
            vSize="count",
            type="index"
            )
dev.off()
```

## Get SC2 data w/ lineages and counts

```bash
DIR='/scratch_slow/sc2/global/2021-04-11/clades/'
DATE=2021-05-09
awk 'BEGIN{FS=","};{print $2}' $DIR/clades-pangolin.csv | grep -v lineage | sort | uniq -c | awk '{print $2"\t"$1}' > ${DATE}_sc2-pangolin.tsv
```

Format of the `sc2-pangolin.tsv` to work with the folling R code should be (no header column):

```
B.1.1.7    24
P.1        3
A.27       12
```

```R
# library
library(treemap)
 
data <- read.csv('sc2-pangolin.tsv', sep = "\t", header = F)
date <- '2021-05-09'

# Create data
#lineage <- c("B.1.1.7","B.1.351","A.27")
#count <- c(27,5,9)
#data <- data.frame(lineage,count)

num_lineages <- length(data$V1)
num_sequences <- sum(data$V2)

# treemap
pdf(paste(date, '_treemap.pdf', sep = ''))
treemap(data,
            index='V1',
            vSize='V2',
            type="index",
            fontsize.labels=c(10),                # size of labels. Give the size per level of aggregation: size for group, size for subgroup, sub-subgroups...
            #fontcolor.labels=c("white","orange"),    # Color of labels
            fontface.labels=c(1),                  # Font of labels: 1,2,3,4 for normal, bold, italic, bold-italic...
            bg.labels=c("transparent"),              # Background color of labels
            align.labels=list(
                #c("center", "center") 
                c("left", "top")
            ),                                   # Where to place labels in the rectangle?
            overlap.labels=0.5,                      # number between 0 and 1 that determines the tolerance of the overlap between labels. 0 means that labels of lower levels are not printed if higher level labels overlap, 1  means that labels are always printed. In-between values, for instance the default value .5, means that lower level labels are printed if other labels do not overlap with more than .5  times their area size.
            inflate.labels=F,                        # If true, labels are bigger when rectangle is bigger.
            palette = "Set2",                        # Select your color palette from the RColorBrewer presets or make your own.
            title=paste("Occurence of SARS-CoV-2 lineages in Germany, ", date " (", num_sequences, " sequences & ", num_lineages, " different lineages)", sep=""),  # Customize your title
            fontsize.title=9,                       # Size of the title
        )
dev.off()
```

## R treemap package with interactive HTML

* https://www.r-graph-gallery.com/237-interactive-treemap.html

