# Signals and Slots #
**<h2>Table of contents</h2>**
   [Introduction](#Introduction)
      [1-Traffic light](#1-Traffic light)
      [2-Calculator](#2-Calculator)
**<h2>Introduction</h2>**

Signals and slots is a language construct introduced in Qt[1] for communication between objects which makes it easy to implement the observer pattern while avoiding boilerplate code. The concept is that GUI widgets can send signals containing event information which can be received by other widgets / controls using special functions known as slots. This is similar to C/C++ function pointers, but signal/slot system ensures the type-correctness of callback arguments.
**<h2>1-Traffic light </h2>**
In this exercise, we will use the QTimer which is a class that  provides repetitive and single-shot timers to simulate a traffic light.
