java c1.Project Temperature Controlled Fan

The temperature controlled fan activates or turn off a fan determined by the ambient temperature. Refer to the appendix for the circuit.

Figure 1-1

2.Code Development Part 1

2.1I/O Initialisation

Create the I/O initialization routine.

Refer to the schematic to determine which are the I/O that are inputs and outputs using the DDRx registers. Digital input ports must be set to logic ‘0’.

For I/O that are analog, their digital function must be disabled using the DIDRO register. Their logic needs to be set to ‘0’.	void ioInit(){
	DDRB =
	DDRC =
	DDRD = 
	PORTB = 
	PORTC =
	PORTD =
	DIDR0 = 
}

Listing 2-1

2.2	3-Digit Display

The concept of driving the 3-digit multiplexing display is shown in Figure 2-1.

A timer interrupt determines the rate of multiplexing. The displayNow( ) is call whenever the timer interrupt. The global variable dpNum stores the 3-digit number to be displayed. The global variable whichDigit determines which digit to turn on. Example, if whichDigit is “2” and dpNum is “295”. The displayNow( ) function when called, will display “9” on the 2nd digit (display the 10th). The displayNow( ) function also maintains the content inside whichDigit to between 1 to 3.

Listing 2-2 shows an example of how the displayNow( ) function can be written.
	

unsigned int dpNum;	// Declare these 2 variables as global.
unsigned char whichDigit;

void displayNow(){
	PORTB = 			// Turn off all mux transistors.
	PORTC = 			// Clear the 4 bit BCD output ports to 0.

	switch(whichDigit){
		case 1:	PORTC = (PORTC  0xF0) | (dpNum/100);// Display 100th.
							PORTB = (PORTB  0xF8) | 0x01	// Turn on mux 1 transistor.
 							break;
		case 2:				// Complete case 2  3. 

		case 3:

	}
	whichDigit++;	// Setup for next digit. 
	if (whichDigit>3) whichDigit = 1;	// Set back to 1st digit.
}

Listing 2-2


2.3	Timer 0  Interrupt

Program timer 0 to interrupt  at an interval of 5ms.  The displayNow( )  function is  called every interrupt. This will take care of the multiplexing display of the content in variable displayNumber.

void interruptInit(){
	// Run timer 0 with prescaler 1024.
	TCCR0B = 
	// Enable timer 0 interrupt.
	TIMSK0 = 
}
	ISR(TIMER0_OVF_vect) {
	// Load timer for 5 ms overflow.
	TCNT0 = 
	displayNow();
}
Listing 2-3

2.4	Test Your Multiplexing Display

Test your program. You should see the 3-digit LED display showing the number in the variable displayNumber.	#include 
#include 

// Declare your global variables here.

// Place all supporting functions here.

int main(){
	ioInit();
	interruptInit();
	// Place the Global Interrupt enable
	//	instruction here.
	while(1){
		// Nothing need to be done
		// in this while-loop.
	}

Listing 2-4

3.Code代 写data、C++，Python
代做程序编程语言 Development Part 2

3.1	ADC

Create 2 functions: (1) readADC( )  and (2) initADC( ). Merge this program with the codes you have developed in Code Development Part 1.  Test your ADC acquisition by reading the temperature sensor into a variable (tempInADC) and pass the content to the variable dpNum. The content in the dpNum variable will be displayed on the 3-digit LED display. Note that the display number is the ADC value and not the temperature.





unsigned int readADC(){
	// Select ADC channel to read.
	ADMUX = 
	// Start ADC conversion.
	ADCSRA = 
	// Wait for ADC conversion complete.
	- - - - 
	// Reset ADC complete flag.	
	- - - -
	// Return converted value.
	- - - -
}
	void initADC(){
	// Enable ADC, clear ADC conversion flag
	//   set ADC clock.
	ADCSRA = 
	// Set up reference voltage.
	ADMUX = 
}

int main(){
	- - - -
	// Merge with the codes in Listing 2-4.
	- - - -
	- - - -
	initADC();
	while(1){
		tempInADC = readADC();
   dpNum = tempInADC;
	}
}

Listing 3-1

3.2	Convert ADC Reading to Temperature

Modify the program in Listing 3-1 to display temperature instead of ADC value. Add a  conversion function to convert the temperature in ADC value to oC before passing the data to dpNum for display. If your hardware is capable of displaying the decimal point, you should program it.


unsigned int readADC(){
	- - - - -
	- - - - -
}


unsigned int convert(unsigned int k){

	// Perform the conversion of ADC value
	// to temperature value here and store
	// the final value in temperature.

	return(temperature);
}
	
int main(){
	- - - -	
	- - - -	
	while(1){
		tempInADC = readADC();
   dpNum = convert(tempInADC);
	}
}

Listing 3-2

4.Code Development Part 3

4.1 	Activating the Fan

Define the temperature to turn on and off the fan. In the main( ) function, use decision making instruction such as if-statement to decide when to turn on or off the fan.

#include 
#include 
#define TurnOnTemp 260
#define TurnOffemp 250	int main(){
	- - - -
	- - - -
	while(1){
   tempInADC = readADC();
   dpNum = convert(tempInADC);		

		// If temperature greater or equal
		// TurnOnTemp, activate the fan.
		// If temoerature less or equal to
		// TurnOffTemp, off the fan.
	}
}

Listing 4-1
5.Additional Features

Notice that the following resources on the circuit*1 which are not used in the above coding:

1.Potentiometer, VR1
2.LED. D2
3.A set of pusb buttons, SW1 to SW3

What other features can you add to your “product”?

*1  Referring to the project circuit. If you are using the trainer circuit, the resources are different.
Appendix A: Project Board Schematic

Appendix B: Trainer Schematic

END OF PAPER

         
加QQ：99515681  WX：codinghelp  Email: 99515681@qq.com
