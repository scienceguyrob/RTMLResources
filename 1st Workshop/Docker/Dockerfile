##############################################################################################
# Purpose   : Dockerize Components of a simple data science stack (CPU version).
# Python    : 3.6
# Version   : 0.1
# Author    : Rob Lyon (robert.lyon@manchester.ac.uk)
##############################################################################################
#
# This docker file will setup an environment with only a basic data science stack.
# See the container maintained by Kamil Kwiek <kamil.kwiek@continuum.io>, from which I
# created this dockerfile:
#
# https://github.com/ContinuumIO/docker-images/blob/master/anaconda/Dockerfile
##############################################################################################

# Use well supported Ubuntu distribution.
FROM ubuntu:16.04

# Contact me for help!
MAINTAINER robert.lyon@manchester.ac.uk


# The WORKDIR instruction sets the working directory for any RUN, CMD, ENTRYPOINT,
# COPY and ADD instructions that follow it in the Dockerfileold. 
WORKDIR /home

#####################################################################
# Make required directories
#####################################################################

# Now create some directories to store files and scripts
RUN mkdir scripts
RUN mkdir soft
RUN mkdir workspace

#####################################################################
# Install System tools needed
#####################################################################

RUN apt-get update \
  && apt-get install -y \
  wget \
  unzip \
  bzip2 \
  software-properties-common \
  python-setuptools

#####################################################################
# Install Anaconda (python 2.7 version)
#####################################################################

# Change to the soft (aka software directory).
WORKDIR /home/soft

# Now install Anaconda to manage the python environment. This installs numpy, scipy, matplotlib,
# jupyter etc. Type conda list inside the built docker container, to view the python modules already
# installed.

RUN wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
RUN bash Miniconda3-latest-Linux-x86_64.sh -b -p /home/soft/anaconda
RUN rm Miniconda3-latest-Linux-x86_64.sh
ENV PATH /home/soft/anaconda/bin:$PATH

#####################################################################
# Update Anaconda (extra data science tools)
#####################################################################

# Python packages from conda. Here libgcc is needed to run XGBoost,
# whilst GCC is needed for neural network based tools.
RUN conda install -y \
    numpy \
    scipy \
    matplotlib \
    scikit-learn \
    scikit-image


RUN conda install -c conda-forge tensorflow \
    certifi \
    imbalanced-learn \
    pydicom \
    nibabel

RUN conda install -c simpleitk simpleitk

# TensorBoard
EXPOSE 6006
# IPython
EXPOSE 8888

#####################################################################
# Finally...
#####################################################################

# Ensure the OS is updated.
RUN apt-get update

# Define the command that will be executed when docker runs the container.
WORKDIR /home
ENTRYPOINT /bin/bash