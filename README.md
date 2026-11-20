# nf-pooled-cellpainting Cellprofiler Pipelines

CellProfiler pipelines for use with [nf-pooled-cellpainting](https://github.com/broadinstitute/nf-pooled-cellpainting/issues/93) workflow.
CP (for Cell Painting) is the prefix used for the phenotyping arm.
BC (for barcoding) is the prefix used for the barcoding arm.

## illum

Ilum pipelines are optional. The workflow can automatically create them for you based on the channel metadata in your samplesheet. They are the same whether you use the default or the stitch-align-crop workflow. They are compatible with `--distributeillum well` but for distribution by channel you must use auto-generated pipelines.

CP_illum needs a `Resize`-`CorrectIlluminateCalculate`-`Resize` block and a `SaveImages` for each channel.

BC_illum needs a `Resize`-`CorrectIlluminateCalculate`-`Resize` block and a `SaveImages` for each channel.

## Apply illum

Illum Apply pipelines apply the illumination correction and, depending on the workflow, aligns images.

| Entrypoint | Arm | Multicycle | Pipeline to use |
| --- | --- | --- | --- |
| default | phenotyping | no | CP_ApplyIllum_NoAlign |
| default | phenotyping | yes | CP_ApplyIllum_Align |
| default | barcoding | yes | BC_ApplyIllum_Align |
| stitch-align-crop | phenotyping | no | CP_ApplyIllum_NoAlign |
| stitch-align-crop | phenotyping | yes | CP_ApplyIllum_NoAlign |
| stitch-align-crop | barcoding | yes | BC_ApplyIllum_NoAlign |

## Segmentation check

Segmentation methods and settings will vary based on phenotyping channels and cell type. The following pipelines offer suggested starting points though you will need to customize to achieve best accuracy on your own images.

| Standard stains | Cell type | Uses plugin | Pipeline to use |
| --- | --- | --- | --- |
| Yes | HeLa - cervical cancer | No | SegCheck_HeLa or SegCheck_HeLa_2 |
| Yes | iPSC - induced pluripotent stem cell | No | SegCheck_iPSC |
| Yes | NPC - neural progenitor | No | SegCheck_NPC |
| Yes | RPE - retinal pigment epithelia | No | SegCheck_RPE |
| Yes | A549 - alveolar basal epithelia | No | SegCheck_A549 |
| Yes | U2OS - osteosarcoma | No | SegCheck_U2OS |

## Barcode preprocessing

You will need to confirm that the barcode segmentation is good and tune color compensation (4 color) or thresholding (2/3 color) for quality barcode calling.

| 2/3/4 color chemistry | Cycles | Nuclear/Cytoplasmic | Multiple iBARs | Realign images | Pipeline to use |
| --- | --- | --- | --- | --- | --- |
| 4 | 12 | Cytoplasmic | No | No | Preprocess_1 |
| 3 | 12 | Nuclear | Yes - 7/5 | Yes | Preprocess_2 |
| 3 | 12 | Nuclear | Yes - 5/7 | Yes | Preprocess_3 |

## Analysis

You will need to edit the segmentation in these pipelines to match the parameters you have worked out during segmentation check. You will need to edit the measurement modules to match all of your phenotpying channels.

| Calls barcodes or ingests calls | 2/3/4 color chemistry | Cycles | Nuclear/Cytoplasmic | Multiple iBARs | Realign images | Pipeline to use |
| --- | --- | --- | --- | --- | --- | --- |
| Calls barcodes | 4 | 12 | Cytoplasmic | No | Yes | Analysis_1 |
| Ingests calls | any | any | any | any | No | Analysis_2 |
