# pyannote + docker = <3

## `pyannote/base`

This is the base image for all others `pyannote` images.  
It includes `pyannote.core` and the following tools:

- scientific libraries
     * numpy
     * scipy
     * jupyter
     * pandas
- computer vision libraries
     * OpenCV
     * dlib
     * scikit-image
- machine learning libraries
     * scikit-learn
     * torch
- audio processing libraries
     * Yaafe

Default command is `jupyter notebook --port=8888 --no-browse --ip=0.0.0.0` ran from `/notebook` work directory.
