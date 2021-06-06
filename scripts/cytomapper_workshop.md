This script serves as an example to highlight the usage of the
[cytomapper](https://www.bioconductor.org/packages/release/bioc/html/cytomapper.html)
R/Bioconductor package. It was written for the [Indiana O’Brien Center
Microscopy
Workshop](http://static.medicine.iupui.edu/obrien/2021Schedule.pdf) in
June 2021.

## Prerequisites

To use this script, first you will need to [install
R](https://www.r-project.org/) and install the `cytomapper` package via:

    if (!requireNamespace("BiocManager", quietly = TRUE))
        install.packages("BiocManager")

    BiocManager::install("cytomapper")

For convenience, please open the provided `cytomapper_demos.Rproj` file.
This will correctly set the working directory.

## Reading in example data

In the first instance, we will read in the example data provided in this
repository. To read in images, the `cytomapper` package provides the
`loadImages` function.

    library(cytomapper)

    # Read in multi-channel images
    images <- loadImages("../data/images/", pattern = ".tiff")

    # Read in masks
    masks <- loadImages("../data/masks/", pattern = ".tiff", as.is = TRUE)

    # Read in single-cell data
    sce <- readRDS("../data/sce.rds")

## Add needed metadata

    # Add channel names
    channelNames(images) <- rownames(sce)

    # Add image name to metadata
    mcols(images) <- mcols(masks) <- DataFrame(ImageName = c("E30", "G23", "J01"))

## Visualize multi-channel images

    plotPixels(image = images,
               colour_by = c("PIN", "CD4", "CD8a"), 
               colour = list(PIN = c("black", "yellow"),
                             CD4 = c("black", "blue"),
                             CD8a = c("black", "red")),
               bcg = list(PIN = c(0, 10, 1),
                          CD4 = c(0, 8, 1),
                          CD8a = c(0, 10, 1)),
               image_title = list(text = c("Non-diabetic",
                                           "Recent onset T1D",
                                           "Long duration T1D")),
               scale_bar = list(length = 100,
                                label = expression("100 " ~ mu * "m")))

![](cytomapper_workshop_files/figure-markdown_strict/unnamed-chunk-2-1.png)

## Visualize segmented cells

    cur_sce <- sce[,sce$CellType %in% c("beta", "alpha", "delta", "Tc", "Th")]

    plotCells(mask = masks,
              object = cur_sce,
              cell_id = "CellNumber",
              img_id = "ImageName",
              colour_by = "CellType",
              image_title = list(text = c("Non-diabetic",
                                          "Recent onset T1D",
                                          "Long duration T1D"),
                                 colour = "black"),
              scale_bar = list(length = 100,
                               label = expression("100 " ~ mu * "m"),
                               colour = "black"),
              missing_colour = "white",
              background_colour = "gray")

![](cytomapper_workshop_files/figure-markdown_strict/unnamed-chunk-3-1.png)

## Outline cells on images

    plotPixels(image = images,
               object = cur_sce,
               mask = masks,
               cell_id = "CellNumber",
               img_id = "ImageName",
               colour_by = c("PIN", "CD4", "CD8a"), 
               outline_by = "CellType",
               colour = list(PIN = c("black", "yellow"),
                             CD4 = c("black", "blue"),
                             CD8a = c("black", "red")),
               bcg = list(PIN = c(0, 10, 1),
                          CD4 = c(0, 8, 1),
                          CD8a = c(0, 10, 1)),
               image_title = list(text = c("Non-diabetic",
                                           "Recent onset T1D",
                                           "Long duration T1D")),
               scale_bar = list(length = 100,
                                label = expression("100 " ~ mu * "m")),
               thick = TRUE)

![](cytomapper_workshop_files/figure-markdown_strict/unnamed-chunk-4-1.png)

## Gate cells on images

    if (interactive()) {
        cytomapperShiny(sce, mask = masks, image = images, 
                        cell_id = "CellNumber", img_id = "ImageName")
    }

## Session info

    sessionInfo()

    ## R version 4.1.0 (2021-05-18)
    ## Platform: x86_64-apple-darwin17.0 (64-bit)
    ## Running under: macOS Catalina 10.15.7
    ## 
    ## Matrix products: default
    ## BLAS:   /Library/Frameworks/R.framework/Versions/4.1/Resources/lib/libRblas.dylib
    ## LAPACK: /Library/Frameworks/R.framework/Versions/4.1/Resources/lib/libRlapack.dylib
    ## 
    ## locale:
    ## [1] en_US.UTF-8/en_US.UTF-8/en_US.UTF-8/C/en_US.UTF-8/en_US.UTF-8
    ## 
    ## attached base packages:
    ## [1] parallel  stats4    stats     graphics  grDevices utils     datasets 
    ## [8] methods   base     
    ## 
    ## other attached packages:
    ##  [1] cytomapper_1.4.1            SingleCellExperiment_1.14.1
    ##  [3] SummarizedExperiment_1.22.0 Biobase_2.52.0             
    ##  [5] GenomicRanges_1.44.0        GenomeInfoDb_1.28.0        
    ##  [7] IRanges_2.26.0              S4Vectors_0.30.0           
    ##  [9] BiocGenerics_0.38.0         MatrixGenerics_1.4.0       
    ## [11] matrixStats_0.59.0          EBImage_4.34.0             
    ## 
    ## loaded via a namespace (and not attached):
    ##  [1] viridis_0.6.1          viridisLite_0.4.0      svgPanZoom_0.3.4      
    ##  [4] shiny_1.6.0            assertthat_0.2.1       highr_0.9             
    ##  [7] sp_1.4-5               GenomeInfoDbData_1.2.6 vipor_0.4.5           
    ## [10] tiff_0.1-8             yaml_2.2.1             pillar_1.6.1          
    ## [13] lattice_0.20-44        glue_1.4.2             digest_0.6.27         
    ## [16] RColorBrewer_1.1-2     promises_1.2.0.1       XVector_0.32.0        
    ## [19] colorspace_2.0-1       htmltools_0.5.1.1      httpuv_1.6.1          
    ## [22] Matrix_1.3-3           pkgconfig_2.0.3        raster_3.4-10         
    ## [25] zlibbioc_1.38.0        purrr_0.3.4            xtable_1.8-4          
    ## [28] fftwtools_0.9-11       scales_1.1.1           svglite_2.0.0         
    ## [31] HDF5Array_1.20.0       jpeg_0.1-8.1           later_1.2.0           
    ## [34] BiocParallel_1.26.0    tibble_3.1.2           generics_0.1.0        
    ## [37] ggplot2_3.3.3          ellipsis_0.3.2         magrittr_2.0.1        
    ## [40] crayon_1.4.1           mime_0.10              evaluate_0.14         
    ## [43] fansi_0.5.0            beeswarm_0.4.0         shinydashboard_0.7.1  
    ## [46] tools_4.1.0            lifecycle_1.0.0        stringr_1.4.0         
    ## [49] Rhdf5lib_1.14.0        munsell_0.5.0          locfit_1.5-9.4        
    ## [52] DelayedArray_0.18.0    compiler_4.1.0         systemfonts_1.0.2     
    ## [55] rlang_0.4.11           rhdf5_2.36.0           grid_4.1.0            
    ## [58] RCurl_1.98-1.3         rhdf5filters_1.4.0     htmlwidgets_1.5.3     
    ## [61] bitops_1.0-7           rmarkdown_2.8          codetools_0.2-18      
    ## [64] gtable_0.3.0           abind_1.4-5            DBI_1.1.1             
    ## [67] R6_2.5.0               gridExtra_2.3          knitr_1.33            
    ## [70] dplyr_1.0.6            fastmap_1.1.0          utf8_1.2.1            
    ## [73] stringi_1.6.2          ggbeeswarm_0.6.0       Rcpp_1.0.6            
    ## [76] vctrs_0.3.8            png_0.1-7              tidyselect_1.1.1      
    ## [79] xfun_0.23
