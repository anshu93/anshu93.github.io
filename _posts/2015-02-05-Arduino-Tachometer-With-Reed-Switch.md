---
layout: post
title:  "Arduino Tachometer with Reed Switch"
date:   2015-02-05 16:34:04
categories: bicycle tachometer
--- 

[Code is hosted at [Github][code]]

<h1> Intro </h1>

One of the big components of the bicycle embedded system is being able to measure a rider's cadence (the RPM of the pedals). To also be able to accurately determine distance, it will be helpful to also know the RPM of one of the wheels of the bike and thus two of these will be used in the final product.

The components used are (vendors sites are linked):

* [Neodymium magnets][magnets]
* [Reed Switch][reed-switch] 

<h1> The Reed Switch </h1>

A reed switch that is functionally equivalent to a simple pushbutton that is activated by a strong enough magnetic field ([wiki][reed-wiki]). Reed switches can be Normally Open (NO) or Normally Complete (NC) which means it is by default open in the former and closed in the latter. 

The reed switch can be wired up as shown in the figure below with one end in ground and the other in PIN 2 of the Arduino. The internal pins of the Arduino have pull-up resistors which means their default value is 'HIGH'. Assuming the reed switches are 'NO', then PIN 2 will by default read HIGH and when the switch closes under a magnetic field, PIN 2 will read LOW.

<div style="width:image width px; font-size:80%; text-align:center;"><img src="/images/Reed_Switch.png" alt="alternate text" width="width" height="height" style="padding-bottom:0.5em;" /></div>
<br>

<h1> Mounting the Switch and the Magnets </h1>

I managed to find an old wheel lying around and borrowed a wheel mount from my friend and put together a setup shown in the picture below:

The magnets were stuck to the rim and the reed switch held with the help of cardboard about an inch away from the rim. You can test to see if that's the appropriate distance by simply spinning the wheel slowly and making sure you hear a clicking noise when the magnets pass the switch.

<img src="/images/wheel_mount.JPG" width="50%" align="middle"/>
<br>

<h1> Code </h1>

The code is rather simple and has the following setup:

* Initialize variables
* Setup Interrupts
* Interrupt handler
* Calculate RPM in main loop

The code can be found on [Github][code].

<h15><b> Initialize Variables: </b></h15>

{% highlight C linenos%}
int sense0 = 2;	// Reed switch pin
volatile byte counter0 = 0;	// Counter
unsigned long lastDebounce0 = 0;	// Time of last observed state change
unsigned long debounceDelay = 250;  // Ignore bounces under 250 ms
float rpm = 0;	// RPM
unsigned long timeold = 0;	// Time of last measurement

{% endhighlight %}

The first variable is the pin name which will be used during assignment. The second is a counter that is declared as a 'volatile' as it is changed in an interrupt service routine (ISR) which is asynchronously fired. The reason this cannot be a simple int is that the compiler will notice that its value is not being changed in the regular code flow and will eliminate what it deems to be 'extraneous' code. The 'volatile' keyword stops the compiler from optimizing the ISR away. The 3rd and 4th variables have to do with debouncing (mechanical oscillation of a switch leading to false readings). 'rpm' and 'timeold' are self-explanatory.

<h15><b> Setup Interrupts: </b></h15>

{% highlight C linenos%}
attachInterrupt(0, magnet, FALLING);

{% endhighlight %}

An interrupt is something that can asynchronously point the processor to another block of code which will execute and return to the previous code flow. The attachInterrupt() function of the Arduino takes in the interrupt number (in this case 0 which corresponds to digital PIN 2), the function that will be run and on what edge of the signal the interrupt will be triggered. As the magnet comes towards the switch, the value on PIN 2 goes from 1 to 0 and thus the magnet() function is called on the leading (falling) edge.

<h15><b> Interrupt Handler: </b></h15>

{% highlight C linenos%}
void magnet() {
  if( (millis() - lastDebounce0) > debounceDelay){
    counter0++;
    lastDebounce0 = millis();
  }
}
{% endhighlight %}

The ISR first checks if it is running beyond the debounce window of (250ms). Thus any change of state between 0-250ms after a change of state is ignored as debouncing. Once it is verified as a valid trigger, the counter is incremented and the time of measurement recorded.

<h15><b> Calculate RPM in main loop: </b></h15>

{% highlight C linenos%}
void loop() {
  if(counter0 > 5){
    float time_elapsed = 1000/ (float) (millis() - timeold);
    rpm = time_elapsed * 60 * (float) counter0;
    timeold = millis();
    counter0 = 0;
    Serial.print("rpm: ");
    Serial.println(rpm);  
  }
}
{% endhighlight %}

If the wheel has rotated more than 5 times, the time it took is recorded and a simple calculation done to calculate rpm. This is then printed to the Serial monitor for debugging. If the number is increased from 5, the rpm will be averaged over more revolutions but will update slowly whereas if it is lowered from 5, it will update quickly but not be too useful.

This code is extensible to IR based systems as well and can be used for multi-blade applications as long as the calculation is modified.

[Code is hosted at [Github][code]]

[code]: https://github.com/anshu93/BicycleArduino/blob/master/rpm/rpm
[magnets]:	http://www.amazon.com/Neodymium-Magnets-inch-Disc-N48/dp/B001KV38ES/ref=sr_1_1?ie=UTF8&qid=1423125734&sr=8-1&keywords=neodymium+magnet
[reed-switch]:	https://www.sparkfun.com/products/8642
[reed-wiki]:	http://en.wikipedia.org/wiki/Reed_switch