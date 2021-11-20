# Signals and Slots #
**<h2>Table of contents</h2>**
   [Introduction](#Introduction)

**<h2>Introduction</h2>**
Signals and slots is a language construct introduced in Qt[1] for communication between objects which makes it easy to implement the observer pattern while avoiding boilerplate code. The concept is that GUI widgets can send signals containing event information which can be received by other widgets / controls using special functions known as slots. This is similar to C/C++ function pointers, but signal/slot system ensures the type-correctness of callback arguments.
## Traffic light ##
