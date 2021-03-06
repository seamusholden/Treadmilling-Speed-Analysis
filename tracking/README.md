# Tracking Fluorescent Speckles with TrackMate

Here, we provide a script to run TrackMate for mutiple files at once without using the GUI: `track_growth_shrink_batch.py`. On a first approach to this routine, TrackMate GUI should be used for single movies to identify the optimal parameters for detecting, tracking and linking the trajectories of fluorescent speckles. Once the parameters for a given experimental setup are defined, this macro provides a highly time-efficient routine to identify and track hundreds of speckles at once. A detailed documentation how to use TrackMate can be found in (Tinevez et al., 2016) as well as online (https://imagej.net/TrackMate). Nevertheless, we briefly describe here our rationale to find the optimal parameters using TrackMate GUI for a single time-lapse movie.

## Using TrackMate GUI 

1.	Open differential movie obtained from step a) in ImageJ (or Fiji).
2.	Run TrackMate GUI (found under the plugins menu)
3.	The first panel displays space and time units from the file metadata. They should be accurate as they were calibrated in the previous step when generating the fluorescent speckles. This is important as everything afterwards will be dependent on this calibration and not on the pixel units anymore.
4.	Select the Laplacian detector (LoG) for particle detection.
5.	Tune the particle diameter using the ‘preview’ tool. Note that a high number of low quality spots are erroneously detected by default. They can be easily discarded increasing the quality threshold (by visual inspection), an arbitrary value estimated from the relative brightness and diameter of each particle. This feature of TrackMate is particularly useful for large data sets as it allows to reduce computation time. Use median filter and do sub-pixel localization. For speckles obtained from FtsZ treadmilling, TrackMate achieves robust detection using a diameter of 0.8 microns and a threshold of 1. Following the dection process, we set an additional filter to keep only particles with a signal-to-noise ratio higher than 1. 
6.	For trajectory building, we use the Simple Linear Assignment Problem (LAP) tracker algorithm, which requires three parameters: (i) the max linking distance, (ii) the max distance for gap closure and (iii) the max frame gap. The first parameter defines the maximally allowed displacement between two subsequent frames. This value has to be chosen carefully. If set too low, trajectories with large displacement steps get fragmented, while too large values can lead to erroneous linking. The other two parameters consider spot disappearance when building trajectories, which can be caused by focussing issues during data acquisition. The temporal Gaussian filter applied in our preprocessing steps minimizes any gaps in the treadmilling trajectories. Accordingly, we set “max linking distance” to 0.5 and “max frame Gap” to 0. 
7.	After the trajectories are built, we exclude trajectories shorter than six seconds and trajectories with a total displacement smaller than 0.4 µm (half of the particle diameter) to avoid tracking fluorescent speckles not corresponding to treadmilling events.
8.	At this point, TrackMate offers a set of interactive tools to examine spots and tracks, which are useful to evaluate the quality of the tracking process, revisit the procedure and adjust some of the parameters. All trajectories are then exported as an XML file (last panel), which contains all the identified treadmilling tracks as spot positions in time. analysis step.

## Using TrackMate in Batch

1. Open macro `track_growth_shrink_batch.py` in ImageJ (or Fiji) and run it.<br>
2. This macro opens a window to select a directory folder containing growth/shrinkage movies and to set all the parameters described above. Unnecessary filters can be set to zero.<br>
3. TrackMate runs windowless for all files and saves the resulting XML file containing spot coordinates in the same directory. In addition a TrackMAte file named ‘TM’ is generated, which can be loaded into ImageJ using the “Load TrackMate file“ command and allows to revisit the whole analysis process for each file individually. <br>
Additionally, we also include a macro named `track_growth_shrink.py` that allows to run TrackMate for a single-movie without using the GUI, i.e it is just a simplified version of the previous macro that runs for a single input file.

