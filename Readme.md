
# TrackEval
*Code for evaluating object tracking.*

This codebase provides code for a number of different tracking evaluation metrics (including the [HOTA metrics](https://link.springer.com/article/10.1007/s11263-020-01375-2)), as well as supporting running all of these metrics on a number of different tracking benchmarks. Plus plotting of results and other things one may want to do for tracking evaluation.

## Currently implemented metrics:

The following metrics are currently implemented:

 - [HOTA metrics](https://link.springer.com/article/10.1007/s11263-020-01375-2) - **Recommended tracking metric** ([code](trackeval/metrics/hota.py))
 - [CLEARMOT metrics](https://link.springer.com/article/10.1155/2008/246309) (MOTA / MOTP / etc.) ([code](trackeval/metrics/clear.py))
 - [Identity metrics](https://arxiv.org/abs/1609.01775) (IDF1 / IDP / IDR / etc.) ([code](trackeval/metrics/identity.py))

## Currently implemented benchmarks:

 - [MOTChallenge](https://motchallenge.net/) (MOT15/16/17/20) ([code](trackeval/datasets/mot_challenge_2d_box.py))
 - [KITTI Tracking](http://www.cvlibs.net/datasets/kitti/eval_tracking.php) ([code](trackeval/datasets/kitti_2d_box.py))
 - [MOTS](https://www.vision.rwth-aachen.de/page/mots) ([KITTI MOTS](http://www.cvlibs.net/datasets/kitti/eval_mots.php) and [MOTS Challenge](https://motchallenge.net/results/MOTS/)) ([code](trackeval/datasets/mots_challenge.py) and [code](trackeval/datasets/kitti_mots.py))

## HOTA metrics

This code is also the official reference implementation for the HOTA metrics:

*[HOTA: A Higher Order Metric for Evaluating Multi-Object Tracking](https://link.springer.com/article/10.1007/s11263-020-01375-2). IJCV 2020. Jonathon Luiten, Aljosa Osep, Patrick Dendorfer, Philip Torr, Andreas Geiger, Laura Leal-Taixe and Bastian Leibe.*

HOTA is a novel set of MOT evaluation metrics which enable better understanding of tracking behaviour than previous metrics.

For more information check out the following links:
 - [Short blog post on HOTA](https://jonathonluiten.medium.com/how-to-evaluate-tracking-with-the-hota-metrics-754036d183e1) - **HIGHLY RECOMMENDED READING**
 - [IJCV version of paper](https://link.springer.com/article/10.1007/s11263-020-01375-2) (Open Access)
 - [ArXiv version of paper](https://arxiv.org/abs/2009.07736)
 - [Code](trackeval/metrics/hota.py)

## Properties of this codebase

The code is written 100% in python with only numpy and scipy as minimum requirements.

The code is designed to be easily understandable and easily extendable. 

The code is also extremely fast, running at more than 10x the speed of the both [MOTChallengeEvalKit](https://github.com/dendorferpatrick/MOTChallengeEvalKit), and [py-motmetrics](https://github.com/cheind/py-motmetrics) (see detailed speed comparison below).

The implementation of CLEARMOT and ID metrics aligns perfectly with the [MOTChallengeEvalKit](https://github.com/dendorferpatrick/MOTChallengeEvalKit).

By default the code prints results to the screen, saves results out as both a summary txt file and a detailed results csv file, and outputs plots of the results.

## Running the code

The code can be run in one of two ways:

 - From the terminal via one of the scripts [here](scripts/). See each script for instructions and arguments, hopefully this is self-explanatory.
 - Directly by importing this package into your code, see the same scripts above for how. 

## Timing analysis

Evaluating CLEAR + ID metrics on Lift_T tracker on MOT17-train (seconds) on a i7-9700K CPU with 8 physical cores (median of 3 runs):		
Num Cores|TrackEval|MOTChallenge|Speedup vs MOTChallenge|py-motmetrics|Speedup vs py-motmetrics
:---|:---|:---|:---|:---|:---
1|9.64|66.23|6.87x|99.65|10.34x
4|3.01|29.42|9.77x| |33.11x*
8|1.62|29.51|18.22x| |61.51x*

*using a different number of cores as py-motmetrics doesn't allow multiprocessing.
				
```
python scripts/run_mot_challenge.py --BENCHMARK MOT17 --TRACKERS_TO_EVAL Lif_T --METRICS Clear ID --USE_PARALLEL False --NUM_PARALLEL_CORES 1  
```
				
Evaluating CLEAR + ID metrics on LPC_MOT tracker on MOT20-train (seconds) on a i7-9700K CPU with 8 physical cores (median of 3 runs):	
Num Cores|TrackEval|MOTChallenge|Speedup vs MOTChallenge|py-motmetrics|Speedup vs py-motmetrics
:---|:---|:---|:---|:---|:---
1|18.63|105.3|5.65x|175.17|9.40x

```
python scripts/run_mot_challenge.py --BENCHMARK MOT20 --TRACKERS_TO_EVAL LPC_MOT --METRICS Clear ID --USE_PARALLEL False --NUM_PARALLEL_CORES 1
```

## License

TrackEval is released under the [MIT License](LICENSE).

## Contact

If you encounter any problems with the code, please contact [Jonathon Luiten](https://www.vision.rwth-aachen.de/person/216/) (luiten at vision dot rwth-aachen dot de).

## Citing TrackEval

If you use this code in your research, please use the following BibTeX entry.

```BibTeX
@misc{luiten2020trackeval,
  author =       {Jonathon Luiten},
  title =        {TrackEval},
  howpublished = {\url{https://github.com/JonathonLuiten/TrackEval}},
  year =         {2020}
}
```

Furthermore, if you use the HOTA metrics, please cite the following paper

```
@article{luiten2020hota,
  title={HOTA: A Higher Order Metric for Evaluating Multi-Object Tracking},
  author={Luiten, Jonathon and Osep, Aljosa and Dendorfer, Patrick and Torr, Philip and Geiger, Andreas and Leal-Taix{\'e}, Laura and Leibe, Bastian},
  journal={International Journal of Computer Vision},
  pages={1--31},
  year={2020},
  publisher={Springer}
}
```

If you use any other metrics please also cite the relevant papers, and don't forget to cite each of the benchmarks you evaluate on.
