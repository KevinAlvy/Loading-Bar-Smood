# Loading-Bar-Smood
<H3><i>Ass Smood as your ass</i></h3>
<p><h2>Step 1: The Code</h2></p>
The code supplied is heavily commented, but just for clarification, here are the main points to get this running:
The following lines set up the I2C for the liquid crystal display. If your code is not exactly the same, it may still work if it uses the same commands.
<pre>
#include &lt;Wire.h&gt;
#include &lt;LiquidCrystal_I2C.h&gt;
</pre>
This line sets up the LCD with the correct address and sets the size. Depending on your module, the address and size may be different.
<pre>
LiquidCrystal_I2C lcd(0x27, 16, 2);
</pre>
There are 6 byte arrays. In each array, there are 7 bytes and each byte has 5 bits (the 3 leading zeros dont count). These are what is used to display the sections of the progress bar.
<pre>
byte zero[] = {&lt;br&gt;  B00000,
  B00000,
  B00000,
  B00000,
  B00000,
  B00000,
  B00000,
  B00000
};
byte one[] = {
  B10000,
  B10000,
  B10000,
  B10000,
  B10000,
  B10000,
  B10000,
  B10000
};
</pre>

<pre>
byte two[] = {
  B11000,
  B11000,
  B11000,
  B11000,
  B11000,
  B11000,
  B11000,
  B11000
};
</pre>

<pre>
byte three[] = {
  B11100,
  B11100,
  B11100,
  B11100,
  B11100,
  B11100,
  B11100,
  B11100
};
</pre>

<pre>
byte four[] = {
  B11110,
  B11110,
  B11110,
  B11110,
  B11110,
  B11110,
  B11110,
  B11110
};
</pre>

<pre>
byte five[] = {
  B11111,
  B11111,
  B11111,
  B11111,
  B11111,
  B11111,
  B11111,
  B11111
};
</pre>
Moving onto the setup() section, we initialise the LCD and then allocate each byte array to a number
<pre>
lcd.begin();
lcd.createChar(0, zero);
lcd.createChar(1, one);
lcd.createChar(2, two);
lcd.createChar(3, three);
lcd.createChar(4, four);
lcd.createChar(5, five);
</pre>
Once all the above is done, we only need 1 call from our code to display the current progress of the bar. The 3 argumens required are the current number, the total count and the line you want to print to in the LCD.
<pre>
updateProgressBar(i, 100, 1);
</pre>
This calls the following method (version 1 code).
<pre>
void updateProgressBar(unsigned long count, unsigned long totalCount, int lineToPrintOn)&lt;br&gt; {
    double factor = totalCount/80.0;          
    int percent = (count+1)/factor;
    int number = percent/5;
    int remainder = percent%5;
    if(number &gt; 0)
    {
       lcd.setCursor(number-1,lineToPrintOn);
       lcd.write(5);
    }
   
       lcd.setCursor(number,lineToPrintOn);
       lcd.write(remainder);   
 }
 </pre>
 Or this (version 2 code):
 <pre>
 void updateProgressBar(unsigned long count, unsigned long totalCount, int lineToPrintOn)&lt;br&gt; {
    double factor = totalCount/80.0;          
    int percent = (count+1)/factor;
    int number = percent/5;
    int remainder = percent%5;
    if(number &gt; 0)
    {
      for(int j = 0; j &lt; number; j++)
      {
        lcd.setCursor(j,lineToPrintOn);
       lcd.write(5);
      }
    }
       lcd.setCursor(number,lineToPrintOn);
       lcd.write(remainder); 
     if(number &lt; 16)	//If using a 20 character display, this should be 20!
    {
      for(int j = number+1; j &lt;= 16; j++)  //If using a 20 character display, this should be 20!
      {
        lcd.setCursor(j,lineToPrintOn);
       lcd.write(0);
      }
    }  
 }
 </pre>
I have heavily commented the code and give a greater description in the video, but essentially it works out the full and part columns that need to be displayed, from the arguments given and uses those to print the appropriate bar onto the screen, starting from the left hand side and on the line given in the argument. The full width of the screen is used.
<p><h2>Step 2: Possible Changes to Play With</h2></p>
The code supplied just displays simple bars accross the screen, but you could alter the byte arrays to print something different. You could shorten the height (by making the top and bottom bytes all zeros. You could try and make arrows instead?

Also, if you wanted to reduce the widch on the screen, it is also possible.Just reduce the 80.0 given in the code to represent the number of columns. If you want to use 1/2 the screen, this would become 40.0 instead.

If you want the bar the start in column 2 instead of column zero, place an offset into the setCursor commands, so:

lcd.setCursor(number-1,lineToPrintOn)

Would become:

lcd.setCursor((number+2)-1,lineToPrintOn)

And:

lcd.setCursor(number,lineToPrintOn);

Would become:

lcd.setCursor((number+2),lineToPrintOn);

I hope it helps in your project.
