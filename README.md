# FDS-CourseOne-Extra-Laterality
This repository is intended to be used in combination with https://github.com/psychdatascience/FDS-CourseOne. This repository contains a dataframe ('tractmeasures.csv') containing the macro- and micro-structural statistics for 61 white matter tracts across multiple data sources.

## Basic brain anatomy review
In order to fully understand the contents of the dataframe, a little background into brain anatomy might be useful.

So, the brain is divided into multiple **tissue types**. Specifically, there is **gray matter**, which is where all of the neuronal cell bodies live and where computation takes place. You can think of these regions as the "computers" of the brain. Connecting these computers is the **white matter**, which are the insulated axons. These are what this dataframe is describing!

Groups of axons are bundled together and organized into structures called white matter **tracts**, also known as **fascicles**. The insulation of these tracts is comprised of a fatty substance known as myelin! This is important because it allows us to study properties of the water matter in living humans using a methodology known as magnetic resonance imaging (MRI), specificall diffusion MRI.

With diffusion MRI, we are measuring the movement of water molecules throughout the brain. Because water is hydrophobic, and thus doesn't like being in contact with the fatty myelin, water is going to move mostly along the axis of orientation of the white matter fascicles. This property of unequalized movement of water molecules is known as "anisotropy". In regions where there is low myelin density, you'll see water moving in all directions equally (i.e. isotropically).

We can leverage this property to then segment out white matter tracts by finding regions of high anistropy of water movement, and "sewing" together regions where water movement is oriented in a similar direction. We call this process "tractography", and with it we generate structures known as "tractograms".

Each tractogram is made up of individual representations of axonal tissue known as "streamlines". Now, with MRI, we aren't measuring actual axons because we don't have the resolution required for that. However, we can build simulated representations of bundled white matter tissue (i.e. streamlines) that can be used as a proxy for axonal distributions.

From these tractograms, we can then segment out major white matter tracts based on known properties of the tracts, including orientation and where they connect with the gray matter. Currently, there exists multiple algorithms for performing this segmentation. The one used to generate this dataframe found in those repository contains 61 white matter tracts (5 callosal, 28 hemispheric tracts).

Once we have our white matter tracts segmented, we can then compute some useful statistics of the the tracts themselves, including volume and structural integrity. I will describe these measures in the next section!

## Dataframe
The dataframe contained in this repository contains structural information regarding major white matter tracts from over 1500 participants. There exists a few important columns that describe the demographic and nominal information, such as the subject ID, which data source they come from, and other useful information like the participant's age and gender. These columns have the following labels:

```
    subjectID: this is the subject identifyer string given to each participant to anonymize the data
    classID: this is the data source from which the data comes
    structureID: this is the name of the white matter tract
    age: this is the participant's age in years
    gender: this is the participant's identified gender
```

Within the dataframe also exists the structural characteristics of the white matter tracts. These statistics describe both macro-level properties of the white matter tract, such as the tract volume or length, and micro-level properties, such as white matter structural integrity (FA). It is important to distinguish these columns based on whether they are macro-level or micro-level.

Here are the macro-level statistical columns that we are most interested in:

```
    StreamlineCount: the number of streamlines that make up the tract
    volume: the volume (mm^3) of the tract
    avgerageStreamlineLength: the average length of the tract
```

The remaining macro-structural statistical columns aren't of much interest to us.

Here are the micro-level statistical columns that we are most interested in:

```
    fa: fractional anistropy; this is a measure of the structural integrity of white matter tracts. higher the FA, the higher the structural integrity
    md: mean diffusivity; this is a measure of how much water movement there is, and thus a rough measure of how organized tissue is in the white matter
```

Here's what the dataframe will look like:

```
    subjectID,classID,structureID,ad,fa,md,rd,StreamlineCount,volume,avgerageStreamlineLength,streamlineLengthStdev,averageFullDisplacement,fullDisplacementStdev,StreamlineLengthTotal,TotalVolumeProportion,TotalCountProportion,TotalWiringProportion,gender,age,ndi,odi,isovf
    P0259,ping-siemens,anterioFrontalCC,1.5617990523444891,0.5776524666686451,0.8985184320303652,0.5668781212662775,2793.0,35671.0,73.17500943826833,14.141308006938246,39.34722572712534,9.633514532008853,204377.801361084,0.0564710315054243,0.0018619999999998,0.0027773343274639,F,15.75,,,
    P0259,ping-siemens,forcepsMajor,1.7420479158508533,0.6640324335155741,0.9183910904036516,0.5065626743559313,1291.0,28226.0,105.45333765386016,24.706248729092703,45.90043514237255,11.020375381589972,136140.25891113284,0.0446847953595949,0.0008606666666666,0.0018500395439506,F,15.75,,,
    P0259,ping-siemens,forcepsMinor,1.4451599267852595,0.5776024753211051,0.8238980377477932,0.5132670937230488,2612.0,30618.0,83.5293353074907,11.35799047751767,45.86238337813841,12.294189152412454,218178.62382316592,0.0484715887593027,0.0017413333333333,0.0029648767010284,F,15.75,,,
```

## Goals for this data

With this, we have a few goals for y'all!

Here they are, in no particular order:

1. fit a quadratic model in a tract, or multiple tracts, to identify a relationship between the participant's age and the white matter tract's FA
2. compute laterality index (LI) between the left and right hemisphere tracts
3. replicate findings from our work done by Mohammed

## What is laterality?

Laterality, in the context of white matter tract properties, hinges on the idea that the hemisphere's undergo different structural reorganization patterns. So, the structural properties of a tract in the left hemisphere might not follow the same developmental trajectory as one in the right hemisphere. By quantifying the difference between the hemispheres as a ratio between the difference between the hemispheres to the sum of the hemispheres, we can build a normalized index for how left or right lateralized a given tract is. This index is called "laterality".

Here is the common defintion in the field for laterality index:

```
    li = (left - right) / (left + right)
```

Laterality can be computed using any type of statistic, including macro- and micro-level statistics!

The goal of the original project was to identify how tract laterality changes across the lifespan!

### References
1. R.E. Smith, J.-D. Tournier, F. Calamante, A. Connelly. Anatomically-constrained tractography: improved diffusion MRI streamlines tractography through effective use of anatomical information. NeuroImage 62 (2012), pp. 1924–1938. 
2. Bullock D, Takemura H, Caiafa CF, Kitchell L, McPherson B, Caron B, Pestilli F. Associative white matter connecting the dorsal and ventral posterior human cortex. Brain Struct Funct. 2019 Nov;224(8):2631-2660. doi: 10.1007/s00429-019-01907-8. Epub 2019 Jul 24. PMID: 31342157.
3. Yeatman JD, Dougherty RF, Myall NJ, Wandell BA, Feldman HM. Tract profiles of white matter properties: automating fiber-tract quantification. PLoS One. 2012;7(11):e49790. doi: 10.1371/journal.pone.0049790. Epub 2012 Nov 14. PMID: 23166771; PMCID: PMC3498174.
4. Bain JS, Yeatman JD, Schurr R, Rokem A, Mezer AA. Evaluating arcuate fasciculus laterality measurements across dataset and tractography pipelines. Hum Brain Mapp. 2019 Sep;40(13):3695-3711. doi: 10.1002/hbm.24626. Epub 2019 May 20. PMID: 31106944; PMCID: PMC6679767.