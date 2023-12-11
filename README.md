# IndoorTrackingML
This is an additional project to test if precision of simple indoor tracking can be elevated via machine learning algorithms.
This project should add to the existing projects of bkuehnis and support his solution (bkuehnis/beacon-backend + bkuehnis/beacon-frontend).

(Setup description etc are further below after findings section)
==========================Findings==========================

Beacon Modell reccomended: Axaet PC061
Weighted F1-Score for approches:
Testdataset (Open Doors) | Testdataset (Closed Doors)

Gateway E06B09BAC79F (Room 1):
Non-ML:	0.7729 (MMedian)	| 0.9604 (Wiener)
NB:		0.7835 (MMedian)	| 0.9728 (MMedian)
SVC:		0.8214 (Raw)	| 0.9789 (MMedian)
KNN:		0.7878 (Raw)	| 0.9819 (MMedian)
RF:		0.8131 (Raw)	| 0.9819 (MMedian)
XG-Boost:	0.8016 (MMedian)	| 0.9819 (MMedian)
RNN (LSTM):	0.9492 (MMedian)	| 0.9782 (MMean)

Gateway F3499FDED02E (Room 2):
Non-ML: 	0.9107 (Wiener)	| 0.9938 (MMedian)
NB:		0.9262 (MMedian)	| 0.9999 (Kalman)
SVC:		0.9262 (MMedian)	| 0.9877 (Kalman)
KNN:		0.9202 (MMedian)	| 0.9846 (Wiener)
RF:		0.9214 (MMedian)	| 0.9846 (Wiener)
XG-Boost:	0.9238 (MMean)	| 0.9908 (Wiener or Kalman)
RNN (LSTM):	0.9499 (Wiener)	| 0.9999 (Raw or Kalman)

Final Modell recommendation based on average performance on open and closed doors data:
Gateway E06B09BAC79F (Room 1):
RNN (LSTM) with Signalfilter MMean -> Weighted F1-Score: 0.9306
Gateway F3499FDED02E (Room 2):
RNN (LSTM) without Signalfilter (Raw-Data) -> Weighted F1-Score: 0.9731

============================================================

## Setup project
The python notebook is developed on google colabs.
As the path are relativ to the google drive directory it is recommended to copy the whole project to own google dirve directory and launch the python notebook with google colabs.
Otherwise the root_path variable has to be adapted and mount of google drive has to be deactivated (in the first lines of code)

How to run the project / code:
The code has to be executed in order otherwise defined function or other previous preparations are missing.
As it is a procedural code and as it was used to write a paper steps in between were saved in a dictionary to simplify the creation of visualisations of the intermediate steps.

The code snippets are grouped by captions which ruffly explain the code segment (in german) but the code itself is commented in english.

IMPORTANT: If you only want to run the current status quo to replicate the results, you can skip / not run the headings which contain (Optional). (Optional) is formatted as bold and the optional capters are tipically timeconsuming or are used if new data is added.


How to add new data:
As the project used freeware and mongodb is limited to 500 communications by day.
Hook.ubeac.io was used as endpoint to collect the gateway data and export it as json file.
(https://hook.ubeac.io/)
Hook.ubeac.io has no limits on number of communications but depending of numbers of bluethooth traffic the new data overwrites the old data. In my project it has overwritten data after 7 or 15 minutes (depending if tv box was activated)
The json files have to be placed in "\IndoorTrackingML\DATA\JSON" (if path to json file is not changed in code)
The set path which points to this location is set in the capter where the json files are transformed to a csv file.

The caption "Neues Datenset erstellen (Optional)" contains all logic to transform a json file to the required csv file. Important a CSV-File "LabelHelper.csv" is required at "\IndoorTrackingML\DATA\" it has to contain the tags of the room and the timestamp, so that the new data can be labled / allocated to the room.
"SpecificTag" can be ignored in the LabelHelper.csv as with the setup of the project a specific allocation in the room can not be done as it is not possible with a single distance value wihtout any further informations.
