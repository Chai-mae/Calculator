# Signals and Slots #
![WhatsApp Image 2021-11-20 at 13 49 44](https://user-images.githubusercontent.com/93831197/142741729-13f22b32-d29b-4462-87a5-28121db49848.jpeg)

**<h2>Table of contents</h2>**
   [Introduction](#Introduction)
   
   [1-Traffic light](1-Traffic-light)
   
   [2-Digital Clock](#2-Digital-Clock)
   
   [3-Calculator](#3-Calculator)

  
**<h2>Introduction</h2>**

Signals and slots is a language construct introduced in Qt for communication between objects which makes it easy to implement the observer pattern while avoiding boilerplate code. The concept is that GUI widgets can send signals containing event information which can be received by other widgets / controls using special functions known as slots. This is similar to C/C++ function pointers, but signal/slot system ensures the type-correctness of callback arguments.

**<h2>1-Traffic light</h2>**

In this exercise, we will use the ##QTimer## which is a class that  provides repetitive and single-shot timers to simulate a ##traffic light##.

![Screenshot_44](https://user-images.githubusercontent.com/93831197/142742078-1d1857fa-2523-4f4d-9e75-09b42230931f.png)


The first function in this exersice is ##timerEvent()## in which we change the color of the light each 1s here is the code :
                                           
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
The seconde function is named ##KeyPressEvent## in which the color is changed when we click on a button in the keybord and here is the code :
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
The last function it is the same as the first but this time we have different timer her is the code

First method
 ```javascript

void TrafficLight::timerEvent(QTimerEvent *e)
{
    if(redlight->isChecked() && lifeTime==4){
        yellowlight->toggle();
        lifetime=0;
    }
    else if(yellowlight->isChecked()&& lifeTime==1){
        greenlight->toggle();
        lifetime=0;
    }
    elseif(greenlight->isChecked()&& lifeTime==2){
        redlight->toggle();
        lifetime=0;
    }
```

The seconde method we add in createwidgets() function  the colors to a vector called lights acoording to it s timer as follows:
 ```javascript
 lights.append(redlight);
 lights.append(redlight);
 lights.append(redlight);
 lights.append(redlight);
 lights.append(yellowlight);
 lights.append(greenlight);
 lights.append(greenlight);
 index=0;
  ```
  And then we implement the function:
  
   ```javascript

void TrafficLight::timerEvent(QTimerEvent *e)
{   index=(index+1)%7;
    lights[index]->toggle();
}
```

**<h2>2-Digital Clock</h2>**
  ![Screenshot_43](https://user-images.githubusercontent.com/93831197/142742077-f79e5d11-f321-43ad-9cb9-ca225fd62026.png)
  
  The ##Digital Clock## exersise shows how to use QLCDNumber to display a number with LCD-like digits and  also demonstrates how QTimer can be used to update a widget at regular intervals.
  Here is the implementation
 ```javascript  
  void digitalclock::createWidgets()
  {
    l1=new QLCDNumber();
    l1->setDigitCount(2);
    l2=new QLCDNumber();
     l2->setDigitCount(2);
    l3=new QLCDNumber();
     l3->setDigitCount(2);


}
void digitalclock::updateTime(){
    auto T=QTime::currentTime();
    l1->display(T.hour());
    l2->display(T.minute());
    l3->display(T.second());

}
void digitalclock::placeWidgets(){
    QLayout *layout=new QHBoxLayout;
    layout->addWidget(l1);
    layout->addWidget(l2);
    layout->addWidget(l3);
    setLayout(layout);
}
void digitalclock::timerEvent(QTimerEvent *e){
    updateTime();
}
```

**<h2>3-Calculator</h2>**
![Screenshot_42](https://user-images.githubusercontent.com/93831197/142742079-e5a614fe-7db5-4253-bb3b-ab12e127242e.png)


The goal of this exersise is to use Signals and Slots to simulate a basic calculator behavior. The supported operations are *, +, -, /.

