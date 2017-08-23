# EMOP Ocular

## Using EMOP Ocular

    git clone https://github.com/Early-Modern-OCR/ocular.git
    cd ocular
    git checkout emop
    ./make_jar.sh


## EMOP-specific differences from tberg12/master

(Check with `git checkout emop; git diff master`.)

* Project name changed to `ocular-emop`.
* `make_jar.sh` produces a jar with the name `ocular-emop_${VERSION}-with_dependencies.jar`.
* `BasicSingleDocumentEvaluatorAndOutputPrinter.java`
    * Always output comparisons file (this is the one displayed by the EMOP dashboard).
    * Comparisons file includes a “Model diplomatic transcription”.
* `TrainFont` / `Transcribe`
    * Added option `-jkNoGsm`.
        * For simplicity, the EMOP dashboard is set up to always use an `-inputGsmPath`, and (I think) always use `-updateGsm true` in `TrainFont`.  The user can put the `-jkNoGsm` option into the dashboard’s field for arbitrary extra options to override this and tell Ocular not to use a GSM at all.
        * For `TrainFont`, a blank GSM will be written to `outputGsmPath`.
        * (A variable `jkNoGsmOutput` is used internally to re-direct `outputGsmPath` so that errors aren't thrown by the fact that `-updateGsm` will be forced to `false`.)
    * Added option `allowOverlappingLines`
        * The emop dashboard defaults to the non-overlapping line extractor, a simple line extractor designed for v0.3 that slices the page into lines based on a height learned from the page. To implement the more sophisticated line extractor, which allows overlapping lines, use `allowOverlappingLines false'. For this option, line heights are hard-wired into the code, but can overlap.


### Changed files

    $ checkout emop
    $ git diff master --name-only
    build.sbt
    make_jar.sh
    src/main/java/edu/berkeley/cs/nlp/ocular/eval/BasicSingleDocumentEvaluatorAndOutputPrinter.java
    src/main/java/edu/berkeley/cs/nlp/ocular/main/FonttrainTranscribeShared.java
    src/main/java/edu/berkeley/cs/nlp/ocular/main/TrainFont.java
    src/main/java/edu/berkeley/cs/nlp/ocular/main/Transcribe.java
    src/main/java/edu/berkeley/cs/nlp/ocular/train/FontTrainer.java


## Updating EMOP branch with changes to tberg12/master version

    git remote add tberg git@github.com:tberg12/ocular.git  # only the first time
    git checkout master
    git pull tberg master
    git checkout emop
    git merge master
    ./make_jar.sh
