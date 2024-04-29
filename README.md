# ISETBioJandJ

This respository contains data and code used in the paper Nankivil, Cottaris, & Brainard (2024). "Theoretical Impact of Chromatic Aberration Correction on Visual Acuity". Biomedical Optics Express, 15(5), 3265-3284, https://doi.org/10.1364/BOE.516049.

## Data

The data are monochromatic wavefront aberration functions for the 18 subjects considered in the paper (4 mm diameter pupil), along with point spread functions (PSFs) derived from the monochromatic aberrations for various combinations of pupil size a amounts of chromatic aberration, as described in the paper.  The subjects are ordered as in the manuscript, with Subject 1 having the best optical quality, Subject 9 the median, and Subject 18 the worst.
  - The underlying wavefront data are described in the publication Cheng, Barnett, Vilupuru, Marsack, Kasthurirangan, Applegate, & Roorda,(2004). "A population study on changes in wave aberrations with accommodation". Journal of Vision, 4(4), 3, https://doi.org/10.1167/4.4.3. We have used and posted them with the permission of the last author. As described in our paper, we considered only 18 of the subjects studied by Cheng et al.
  - This repository contains only a subset of the data analyzed in our paper. That is because the full set of data is large.  We provide here only the monitor spectral functions and the data for Subject 9 required to run the example program as described below. The full data set, including the wavefront aberration functions may be downloaded here: https://www.dropbox.com/scl/fi/h0oloijo8mptkrzthqmd9/data.zip?rlkey=bu2dhp3oc45es222dfyxk7j0x&dl=1. This is a zip archive of size approximately 100 GB. To use after downloading, you will need to modify the code to point at the unpacked archive as described under code below.
  - The wavefront abberation functions are in directory "data/WaveAberrations".  There is a .csv file for each subject. and a plot is provided of the monochromatic PSF corresponding to each aberration.  The file in that directory, "Zernike data from eye.docx", provides information about the wavefront aberration data, including a reference to the paper in which the measurements were described. **[Need to add information about the spatial sampling in the pupil plane to the document, although one could deduce this from the fact that the pupil was 4mm. Might also explain the code that leads to the filenames - is the first number the defocus that was added to optimize Sthrel? In D or uM? What do the "astig_axis_0" versus "no_atig0" strings denote?"]**
  - The point spread functions are provided as .mat files in directory "data".  The directories "FullVis_PSFs_20nm_SubjectX" give PSFs for a 4 mm pupil for each subject.  Each of these directories has PSFs for each of the 49 LCA/TCA combinations examined in the paper.  Additional directores for each subject have names with string (e.g.) "2p5mmEPD" appended. These are calculated for entrance pupil diameters of 1.5, 2.0, 2.5, 3.0, 3.5, and 4.0 mm as indicated by the string.  The paper briefly describes additional simulations for Subject 9 run at this series of pupil sizes. We provide the PSFs for all LCA/TCA combinations for each subject/pupil size as these may be useful for others wishing to extend our work.  Note that the PSFs in the directories with "4p0mmEPD" appended are for the same 4 mm pupil size as those in the directories without an explict appended EPD string. These two sets of 4 mm PSFs differ slightly because of a change in how an interpolation was implemented in the calculation of the PSFs.
  - Each individual directory contains one file for each polychromatic PSF.  The filename contains strings that give the amount of LCA in diopters (e.g., "LCA_1278" indicates 1.278 D LCA between 700 and 400 nm as described in the paper) and amount of horizontal and vertical foveal TCA (e.g. "Hz1380_TCA_Vt2760_TCA" indictates 1.380 microns of horizontal and 2.7260 microns of vertical TCA), again as between 700 and 400 nm described in the paper.  Also as described in the paper, the amount of horizontal and vertical TCA is paired in each PSF, with the vertical being twice the horizontal.
  - Each .mat file contains a structure, opticsParams, whose fields give the spatial support for the PSF in arcminutes, the wavelength support in nm, the pupil diameter used in calculating the PSF, and the number of microns per degree assumed for the human eye.  It also contains a variable, "thePSFEnsemble"", that is a 3D square array providing the polychormatic PSF. The first index is vertical rows, the second horzontal columns, and the third is wavelength. **[Check horizontal/vertical convention and fix desription if it's backwards.]**
  - The data directory also contains .mat files with the monitor spectra used in the paper simulations. The "BVAMS_White_Background.mat" contains a spectrum of all zeros and indicates the black level of the monitor, despite the word "White" in the filename.  The "BVAMS_White_Guns_At_Max.mat" file gives the monitor channel spectra at max for the low luminance condition, while "BVAMS_White_Guns_At_Max_HL_.mat" gives the channel spectra for the high luminance condition.  Each of these files contains a variable "spd"", where the first column is wavelength in nm and the second is radiance in units of Watts/[sr-m2-nm].  **[Radiance units in monitor channel files. Check that these are the units, and that its not per wavelength band rather than per nm.]**
  - The full set of point spread functions is large, so it takes a while to download the whole repository.
  
## Code

The code allows simulation using ISETBio of ideal observer performance in a 4-AFC tumbling E experiment, using the PSFs and display SPDs provided in the data directory.
  - The top level script is directory "main", alled "runTaskPaper.m". It illustrates calculations for Subject 9. for a subset of the LCA/TCA combinations at 4 mm pupil diameter, low luminance condition as in the paper. This script reproduces the main results of the paper, although the figure format is not production quality and you would have to add additional LCA/TCA conditions to run out all of the paper conditions.  Note that there is small numerical variation run-to-run due to a different sequence of random numbers being used in each run, so the exact results you obtain will differ a little from those in the paper.
  - Comments in this script explain how to add and modify conditions, for example how to reconfigure to run all of the conditions in the paper.
  - The script writes out figures into the directory "figures", and saves output data into directory "results". These top directories are set to be ignored by git, and will be created as needed by the script.  The script also creates subfolders that help track the parameters run.
  - The paper provides various figures that illustrate aspects of methods or intermediate steps in the calculations. Because producing production quality figures is a little fussy, we have not provided code to (or at least not documented how to) produce these ancillary figures.  Some figures that can be produced by the main script are not saved.
  - The directory "test" contains a test script.
  - The directory "toolbox" contains support routines.
  - The directory "sampledata" contains enough data to execute runTaskPaper. To run other conditions, you will need to download the full data set as described under data above, and then modify the line in runTaskPaper that sets the Matlab preference ('ISETBioJandJ','dataDir') to point at the data you downloaded.
	
## Dependencies

The code depends on a set of other publicly available repositories.
  - If you use ToolboxToolbox (https://github.com/toolboxhub/toolboxtoolbox.git), you can download this repository to your projects folder and run tbUseProject('ISETBioJandJ') to fetch other dependencies and put them on your path.  The ToolboxToolbox configuration is in directory "configuration".  As noted below, you will have to obtain the Palamedes Toolbox outside of Toolbox Toolbox.
  - If you do not use ToolboxToolbox, you will need the following on your path.
    - ISETBioCSFGenerator, branch ChromAbPaper, https://github.com/isetbio/ISETBioCSFGenerator.git
    - ISTETBo, branch ChromAbPaper, https://github.com/isetbio/isetbio.git
    - mQUESTPlus, https://github.com/brainardlab/mQUESTPlus.git
    - Palamedes Toolbox, https://palamedestoolbox.org, version 1.8.2. [This may work with more recent versions, but we run against 1.8.2. You may need to write to the Palamedes team to get that version, and because of the way Palamedes is made available, ToolboxToolbox cannot get it for you so you will need to download it.]

