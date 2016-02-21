# Jupyter Notebook with jupyter-scala

[![](https://badge.imagelayers.io/dockoey/jupyter-scala:latest.svg)](https://imagelayers.io/?images=dockoey/jupyter-scala:latest 'Get your own badge on imagelayers.io')

### What you get
  * [Jupyter](http://jupyter.readthedocs.org/en/latest/install.html) based on `python3`
  * [jupyter-scala](https://github.com/alexarchambault/jupyter-scala) `2.11.7` based on [java:8](https://hub.docker.com/_/java/)

### Usage

```shell
docker run --rm -it -p 8888:8888 -v "$(pwd):/notebooks" dockoey/jupyter-scala
```

### Note
  [jupyter-scala](https://github.com/alexarchambault/jupyter-scala) is currently being updated. Read guides for a [new version](https://github.com/alexarchambault/jupyter-scala/tree/topic/update-readme)