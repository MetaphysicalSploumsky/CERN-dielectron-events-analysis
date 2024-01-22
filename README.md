# Determining the Invariant Mass of Particles Using Machine Learning and a Collision Dataset

## Intro 

Hello hello! This repository contains data from dielectron events in pp collisions and my analysis of it. My goal starting this project was to ship a simple but effective model capable of predicting invariant masses from the features of the events. In the end, I built a model which predicted the masses within 1.615 GeV in average; an acceptable result considering the 25.073 GeV standard deviation of my test set. 

## The Data

The dataset contains 100k dielectron events in the invariant mass range 2-110 GeV. It was relased by the CMS collaboration, as part of their education and outreach initiatives. The features are: 

1) Run: The run number of the event.
2) Event: The event number.
3, 11) E1, E2: The total energy of the electron (GeV) for electrons 1 and 2.
4, 5, 6, 12, 13, 14) px1,py1,pz1,px2,py2,pz2: The components of the momemtum of the electron 1 and 2 (GeV).
7, 15) pt1, pt2: The transverse momentum of the electron 1 and 2 (GeV).
8, 16) eta1, eta2: The pseudorapidity of the electron 1 and 2.
9, 17) phi1, phi2: The phi angle of the electron 1 and 2 (rad).
10, 18) Q1, Q2: The charge of the electron 1 and 2.
19) M: The invariant mass of two electrons (GeV).

Source of the dataset: McCauley, Thomas; (2014). https://opendata.cern.ch/record/304

## Technicalities of the Model

My initial attempt consisted in an ensemble of three gradient boosting algorithms: xGBoost, LGBM, and CatBoost. I trained each individually in order to monitor their metrics, then ensembled them and trained a voting model. What I found was that this approach would substantially increase training time, with marginal improvements of the results.

In light of this, I went with CatBoost as standalone model, as it was the best performing of the three. Using Optuna and RandomizedSearchCV, the hyperparameters were tuned to: {'subsample': 0.7, 'l2_leaf_reg': 6.0, 'iterations': 2000, 'depth': 4, 'colsample_bylevel': 1.0}. I also decided to go with a full-dimensional set of features (d = 26), instead of reducing dimensionality. 

## acknowledgements

I would like to underline Danny Bozbay work, as I drew a lot of inspiration from his analysis and learned a lot from his notebook. You can find it all here: https://www.kaggle.com/code/danielbozbay/cern-data-end-to-end-gradient-boosting-0-9-rmse

## Cool Papers 

Here are two insightful papers I read before starting to work, as I wanted to know the "why" and "how" behind dielectron events. I recommend, as a fun activity, or as something that may push you towards the discovery of a lifetime.

- Search for large extra dimensions in dimuon and dielectron events in pp collisions at s=7 TeV, CMS Collaboration et al. Physics Letters B, Volume 711, Issue 1, 2012, Pages 15-34, ISSN 0370-2693, https://doi.org/10.1016/j.physletb.2012.03.029.(https://www.sciencedirect.com/science/article/pii/S0370269312003036)
- Dielectron and heavy-quark production in inelastic and high-multiplicity proton–proton collisions at s=13TeV, Alice Collaboration, Physics Letters B, Volume 788, 2019,Pages 505-518, ISSN 0370-2693, https://doi.org/10.1016/j.physletb.2018.11.009.(https://www.sciencedirect.com/science/article/pii/S0370269318308475)
