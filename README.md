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
group <- c("B.1.1.7","B.1.351","A.27")
value <- c(27,5,9)
data <- data.frame(group,value)
 
# treemap
treemap(data,
            index="group",
            vSize="value",
            type="index"
            )
```

## R treemap package with interactive HTML

* https://www.r-graph-gallery.com/237-interactive-treemap.html

