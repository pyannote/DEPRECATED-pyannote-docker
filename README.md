# pyannote + docker = <3

## `pyannote/base-scientific-python`

 + numpy
 + scipy
 + pandas
 + scikit-learn
 + scikit-image
 + numexpr
 + jupyter
 + matplotlib

 ## `pyannote/base-audio`

  + `pyannote/base-scientific-python`
  + ffmpeg
  + Yaafe

## `pyannote/base-audiovisual`

 + `pyannote/base-audio`
 + OpenCV
 + dlib

## `pyannote/base`

 + `pyannote/base-audiovisual`
 + torch

Default command is `jupyter notebook --port=8888 --no-browse --ip=0.0.0.0` ran from `/notebook` work directory.
