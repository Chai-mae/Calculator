# Signals and Slots #
![WhatsApp Image 2021-11-20 at 13 49 44](https://user-images.githubusercontent.com/93831197/142741729-13f22b32-d29b-4462-87a5-28121db49848.jpeg)

**<h2>Table of contents</h2>**
   [Introduction](#Introduction)
   
   [1-Traffic light] (#1-Traffic-light)
   
   [2-Calculator](#2-Calculator)

  
**<h2>Introduction</h2>**

Signals and slots is a language construct introduced in Qt[1] for communication between objects which makes it easy to implement the observer pattern while avoiding boilerplate code. The concept is that GUI widgets can send signals containing event information which can be received by other widgets / controls using special functions known as slots. This is similar to C/C++ function pointers, but signal/slot system ensures the type-correctness of callback arguments.

**<h2>1-Traffic light</h2>**

In this exercise, we will use the QTimer which is a class that  provides repetitive and single-shot timers to simulate a traffic light.

The first function in this exersice is timerEvent in which we change the color of the light each 1s here is the code :
                                           
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
The seconde function is named KeyPressEvent in which the color is changed when we click on a button in the keybord and here is the code :
 ```javascript
void TrafficLight::keyPressEvent(QKeyEvent *e){
    if(e->key()==Qt::Key_Escape)
       qApp->exit();
   else if(e->key()==Qt::Key_R){
      lights[0]->toggle();
  }
   else if(e->key()==Qt::Key_Y){
       lights[1]->toggle();
   }
   else if(e->key()==Qt::Key_G){
       lights[2]->toggle();
   }
   }
```
