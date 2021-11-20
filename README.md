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

In this exercise, we will use the QTimer which is a class that  provides repetitive and single-shot timers to simulate a traffic light.

![Screenshot_44](https://user-images.githubusercontent.com/93831197/142742078-1d1857fa-2523-4f4d-9e75-09b42230931f.png)


The first function in this exersice is timerEvent() in which we change the color of the light each 1s here is the code :
                                           
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
The seconde function is named KeyPressEvent() in which the color is changed when we click on a button in the keybord and here is the code :
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
  
  The Digital Clock exersise shows how to use QLCDNumber to display a number with LCD-like digits and  also demonstrates how QTimer can be used to update a widget at regular intervals.
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
The Calculator class provides a simple calculator widget. It inherits from QWidget and has several private slots associated with the calculator's buttons.


Our first step is to respond to each digit click
```javascript 
void Calculator::newDigit()
{
    button = dynamic_cast<QPushButton *>(sender());
    int digitValue = button->text().toInt();
    if (disp->intValue() == 0 && digitValue == 0)
        return;

    if (waitingfordigit) {
        disp->display("0");
    waitingfordigit= false;
    }

    disp->display(disp->intValue()*10 + digitValue);
}
```
Pressing one of the calculator's digit buttons will emit the button's clicked() signal, which will trigger the newDigit() slot.
This function will use the Sender method to get the identity of which button was clicked and act accordingly.
The slot needs to consider two situations in particular. If display contains "0" and the user clicks the 0 button, it would be silly to show "00". And if the calculator is in a state where it is waiting for a new operand, the new digit is the first digit of that new operand; in that case, any result of a previous calculation must be cleared first.

Operation Interaction

Now we will move on the operation of the four buttons. We will the same mechanism using the sender method. Hence we will define two slots to handle the click on the operations buttons:addOp() slot for + and - multOp() slot * and /
```javascript
void Calculator::addOp(){

    auto button = dynamic_cast<QPushButton*>(sender());
    operation = new QString{button->text()};
    auto operand = disp->intValue();
    if(!pendingMultOp.isEmpty()){
        if(!calculate(operand, pendingMultOp)){
            Calculator::resetSlot();
            disp->display("Error");
            return;
        }
       disp->display(factorSoFar);
       operand= factorSoFar;
       factorSoFar= 0;
       pendingMultOp.clear();


    }
    if(!pendingAddOp.isEmpty()){
        if(!calculate(operand, pendingAddOp)){
            Calculator::resetSlot();
            disp->display("Error");
            return;
        }
        disp->display(sumSoFar);
        }
    else {
        sumSoFar = operand;
    }
    pendingAddOp = *operation;
    waitingfordigit = true;
}
```

The addOp() slot is called when the user clicks the + or - button.

Before we can actually do something about the clicked operator, we must handle any pending operations. We start with the multiplicative operators, since these have higher precedence than additive operators.

If *or / has been clicked earlier, without clicking = afterward, the current value in the display is the right operand of the *or / operator and we can finally perform the operation and update the display.

If + or - has been clicked earlier, sumSoFar is the left operand and the current value in the display is the right operand of the operator. If there is no pending additive operator, sumSoFar is simply set to be the text in the display.

Finally, we can take care of the operator that was just clicked. Since we don't have the right-hand operand yet, we store the clicked operator in the pendingAddOp variable. We will apply the operation later, when we have a right operand, with sumSoFar as the left operand.

```javascript
void Calculator::multOp(){
    auto button = dynamic_cast<QPushButton*>(sender());
    operation = new QString{button->text()};
    auto operand = disp->intValue();
    if(!pendingMultOp.isEmpty()){
        if(!calculate(operand, pendingMultOp)){
            Calculator::resetSlot();
            disp->display("Error");
            return;
        }
       disp->display(factorSoFar);
      }
    else {
        factorSoFar = operand;
    }
    pendingMultOp = *operation;
    waitingfordigit = true;
}
```

The multOp() slot is similar to addOp(). We don't need to worry about pending additive operators here, because multiplicative operators have precedence over additive operators. Like in addOp(), we start by handing any pending multiplicative and additive operators. Then we display sumSoFar and reset the variable to zero. Resetting the variable to zero is necessary to avoid counting the value twice.

As an enhancement to our calculator we add two buttons which are :

- The reset button which  resets the calculator to its initial state.
```javascript

    void Calculator::resetSlot(){
    sumSoFar = 0;
    factorSoFar = 0;
    waitingfordigit = true;
    operation = nullptr;
    disp->display("0");


}
```
The +/- button which changes the sign of the value in display. If the current value is positive, we prepend a minus sign; if the current value is negative, we remove the first character from the value (the minus sign).

```javascript

void Calculator::changeSign(){
    auto value = disp->intValue();
    if (value > 0){
        value = -value;
    }
    else if (value < 0){
        value= value * -1;
 } disp->display(value);
}
bool Calculator:: calculate(int rightOp, const QString &pendingOp){
    if (pendingOp == tr("+")) {
          sumSoFar += rightOp;
      } else if (pendingOp == tr("-")) {
          sumSoFar -= rightOp;
      } else if (pendingOp == tr("*")) {
          factorSoFar *= rightOp;
      } else if (pendingOp == tr("/")) {
      if (rightOp == 0)
          return false;
      else
           factorSoFar /= rightOp;

      }
      return true;

}
```
The private calculate() function performs a binary operation. The right operand is given by rightOp. For additive operators, the left operand is sumSoFar; for multiplicative operators, the left operand is factorSoFar. The function return false if a division by zero occurs.

```javascript

bool Calculator:: calculate(int rightOp, const QString &pendingOp){
    if (pendingOp == tr("+")) {
          sumSoFar += rightOp;
      } else if (pendingOp == tr("-")) {
          sumSoFar -= rightOp;
      } else if (pendingOp == tr("*")) {
          factorSoFar *= rightOp;
      } else if (pendingOp == tr("/")) {
      if (rightOp == 0)
          return false;
      else
           factorSoFar /= rightOp;

      }
      return true;

}
```
Like in addOp(), we start by handing any pending multiplicative and additive operators. Then we display sumSoFar and reset the variable to zero. Resetting the variable to zero is necessary to avoid counting the value twice.
```javascript

void Calculator::enterbutton(){


auto operand = disp->intValue();
if(!pendingMultOp.isEmpty()){
    if(!calculate(operand, pendingMultOp)){
        Calculator::resetSlot();
        disp->display("Error");
        return;
    }
    operand = factorSoFar;
    factorSoFar=0;
    pendingMultOp.clear();
}
if(!pendingAddOp.isEmpty()){
    if(!calculate(operand, pendingAddOp)){
        Calculator::resetSlot();
        disp->display("Error");
        return;
    }
    pendingAddOp.clear();
}
else{
    sumSoFar= operand;
}
disp->display(sumSoFar);
sumSoFar = 0;
waitingfordigit = true;
}
```
