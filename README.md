# jspsych-vwp-webgazer

This is a boilerplate for Visual World Paradigm experiments using WebGazer.

## Generic documentation

Please read the [generic documentation](https://github.com/UiL-OTS-labs/jspsych-uil-template-docs) for ILS jsPsych templates.

## Preparing your experiment

### Stimuli

To make use of this boilerplate, you have to update the stimuli and the item lists.
For this visual world task, there's a single image file and a single audio file defined per trial. Images should contain the target stimuli arranged in your desired layout. That is to say, if you are preparing a visual world task with two items per trial, then each image file should contain two pictures, properly centered and aligned.

The image files are stored under the `img` folder, while the audio files are stored under `sounds`.

You can get an idea of how the example items are defined by looking in `stimuli.js`. The example contains a list of practice items (`PRACTICE_ITEMS`) and a single list of test items (`LIST_1`). Each item is assigned a unique id, an item type, an image file and a sound file.

The required item types should be defined at the top of `stimuli.js` using constants (`PRACTICE` and `TARGET` in the example.)


### List assignment and randomization

In the likely case that your experiment involves multiple lists, these should be defined as multiple array variables, e.g.:

```
const LIST_1 = [ ... <item definitions go here> ... ];

const LIST_2 = [ ... <items here too> ... ];

const LIST_3 = [ ... <items here too> ... ];
```

Each list should also be assigned a name in the global `LISTS`. The list definitions and names are combined in the global `TEST_ITEMS` which acts as the definitive list, that the experiment template will actually load and interpret.

When the experiment starts, the participant will be assigned one of the lists at random. There are two different supported methods for this random assignment, which are documented on the [datastore server](https://experiment-datastore.lab.hum.uu.nl/help/) in more detail. Choosing the preferred method is done by commenting/uncommenting the relevant snippet in `main.js`, inside the function `main()`.

Once a list has been chosen for the participant, the order of list items (i.e. trials) is randomized. The default randomization method is a pseudo-randomization with a limit on the number of successive items of a single item type. This is configured in `globals.js` using the constants `PSEUDO_RANDOMIZE` and `MAX_SUCCEEDING_ITEMS_OF_TYPE`. Note that based on your lists and chosen value for `MAX_SUCCEEDING_ITEMS_OF_TYPE` a suitable randomization might be too unlikely or impossible. In that case you might have to relax the constraints or introduce extra filler items.

If you prefer to randomize the trials without any constraints, you can use the function `uil.randomization.randomShuffle()` instead of `uil.randomization.randomizeStimuli()`. See [jspsych-uil-utils documentation](https://uil-ots-labs.github.io/jspsych-uil-utils/) for more information.

### Configuration

In the file `globals.js` you will find several configurable settings for the experiment. Specifically for this template, you can configure the placement of calibration points (`CALIBRATION_POINTS`), and whether the participant is required to click on the points for calibration to proceed (`CALIBRATION_CLICK`).

## Output

Per trial, eye tracking data is stored under the key `webgazer_data`. This is a list of xy-coordinates measured in pixels, and a timestamp `t` measured in milliseconds. The time intervals between data points is hardware and browser dependent, and will thus differ between participants. Per-participant however it should be quite consistent.

There is also information saved about the screen geometry of the relevant stimulus, under `webgazer_targets` -> `#image-stimulus`. If the experiment is properly implemented, this should be the same across trials, because images should all have the same size.

Coarser-grained but maybe useful information about the partcipant's screen is also stored in the beginning of the experiment, in the `browser_data` step (see `main.js`).

### Data processing

Included in this reposiotry is an example R script (`webgazer_data.R`) for reading the gaze data for all trials into a single dataframe.
