# Data for demonstrations

This folder contains data of three images highlighted in Figure 1C+D of the [cytomapper publication](https://academic.oup.com/bioinformatics/article/36/24/5706/6050702).
To test the full functionality of the `cytomapper` package, three components are needed:

* `images`: contains three multichannel images (38 channels). Image `E03` was taken of a healthy patient, `G23` was derived from an early onset patient and `J01` wa acquired from a long-duration T1D patient.

* `masks`: contains single-cell segmentations masks to the corresponding images.

* `sce.rds`: contains an single R object in form of a `SingleCellExperiment`. This data class contains all cells corresponding to the above images.

This dataset is a small subset of pancreatic islets acquired in the following publication:

[A Map of Human Type 1 Diabetes Progression by Imaging Mass Cytometry](https://www.sciencedirect.com/science/article/pii/S1550413118306910)


