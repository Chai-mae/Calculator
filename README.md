# Signals and Slots #
![WhatsApp Image 2021-11-20 at 13 49 44](https://user-images.githubusercontent.com/93831197/142741729-13f22b32-d29b-4462-87a5-28121db49848.jpeg)

**<h2>Table of contents</h2>**
   [Introduction](#Introduction)
   
   [1-Traffic light](#1-Traffic light)
   
   [2-Calculator](#2-Calculator)

  
**<h2>Introduction</h2>**

Signals and slots is a language construct introduced in Qt[1] for communication between objects which makes it easy to implement the observer pattern while avoiding boilerplate code. The concept is that GUI widgets can send signals containing event information which can be received by other widgets / controls using special functions known as slots. This is similar to C/C++ function pointers, but signal/slot system ensures the type-correctness of callback arguments.

**<h2>1-Traffic light</h2>**

In this exercise, we will use the QTimer which is a class that  provides repetitive and single-shot timers to simulate a traffic light.
                                           
 ```javascript
void TrafficLight::timerEvent(QTimerEvent *e)
{
    if(redlight->isChecked()){
        yellowlight->toggle();
    }
    else if(yellowlight->isChecked()){
        greenlight->toggle();
    }
    else{
        redlight->toggle();
    }
```
