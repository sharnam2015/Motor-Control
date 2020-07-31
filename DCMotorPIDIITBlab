//This code is for controlling a DC Motor using PID , an XEP MCU, encoders and PWM. 
// This was done as a part of the Microprocessors and Control Lab at IIT Bombay in April 2017
// MAC Lab credits - Prof Prassanna Gandhi , IIT Bombay

#include <hidef.h>      /* common defines and macros */
#include "derivative.h"      /* derivative-specific definitions */

long count=0;
long xDesired=2*7404;
long xMax= 2*7404;
long xOld=0;
int xDot2=0;
int xDot3=0;
int xDotf=0;
long int xDot=0;
int xDot4=0;
int xDot5=0;
int xDot6=0;
int newDuty=0;
int Kp=200;
int Kd=1;
long int Ki=1;
long int sum = 0;
long errorSum=0;
long errorOld=0;

void initPIT(void);
void PWM_for_Motor(void);
void PWM_init(void);
        
int        En_Read=0x0000;        
int        L_DATA=0x00,U_DATA=0x00;        

void main(void) {
  /* put your own code here */
  
  initPIT();
  PWM_init();                
  PWM_for_Motor();

  DDRA=0xFF;        
  DDRB=0x00;

        EnableInterrupts;


  for(;;) {
    _FEED_COP(); /* feeds the dog */
  } /* loop forever */
  /* please make sure that you never leave main */
}


void initPIT(void){
 PITMUX=0x00;
 
 PITCFLMT_PITE=1;
 PITCE_PCE0=1;
 
 PITMTLD0=0b11111111;
 //PITLD0=0b0001111010000011;
 //PITLD0=0b0000000001001101;
 PITLD0=7;
 
 PITINTE_PINTE0=1;
 
 
}

void interrupt 66 testCode(void){
 PITTF_PTF0=1;        

 PORTA=0x00;                                                                                         //Enable=0        ,Select        Pin=0                                                {SEL=PA2,OE=PA3}        
 //PORTA_PA2=0;
 //PORTA_PA3=0;
 
 U_DATA=PORTB; 
                                                                                        
 PORTA=0x04;                                                                                 //Enable=0        ,Select        Pin=1                                                {SEL=PA2,OE=PA3}        
 
// PORTA_PA2=1;
// PORTA_PA3=0;
 
 L_DATA=PORTB;        
 En_Read=(U_DATA  << 8)|L_DATA;                //        Combining        lower                byte        and        Upper                Byte        into                a        16-bit        binary        number        
 PORTA=16;                         //Inhibit        Logic        reset                Enable        Pin=1,        Select        Pin        =        X        
 count++;
           
           
xDot=En_Read-xOld;
           xDotf=(xDot+xDot2+xDot3+xDot4+xDot5+xDot6)/6;
   //xDotf=xDot;
   newDuty=(Kp*(xDesired-En_Read))/xMax +((Ki*(sum))/5000000) +(((xDesired-En_Read)*Ki)/5000000)+ (-Kd*xDot);
 if(newDuty<0){
 // PORTA_PA0= 1;                 //IN B to PA0
 // PORTA_PA1= 0;  
 PORTA=17;               //IN A to PA1
  
  sum=sum+(xDesired-En_Read);
  newDuty=newDuty*-1;
  
  
 }else{
  //PORTA_PA0=0;
  //PORTA_PA1=1;
  PORTA=18;
 }
 PWMDTY5=newDuty;   //change the motor duty to the new value
    xDot6=xDot5;
    xDot5=xDot4;
    xDot4=xDot3;
  xDot3=xDot2;
xDot2=xDot; 

 xOld=En_Read;
 

 if(count%2500==0){
   xDesired=xMax-xDesired;
   sum=0;
 }
 
 
}

//Functions        used        to        generate        PWM        for        motor        and        decoder        clock:        
void        PWM_for_Motor()        {        
        PWMCLK_PCLK5=0;                
        PWME_PWME5=1;        
        PWMPOL_PPOL5=1;                                                                                                                
        PWMPRCLK=0x01;                
        //PWMDTY5=0x80;                                
        PWMPER5=0xFF;                        
}

//PWM for decoder clock
void        PWM_init()        {        
        PWMCLK_PCLK0=0;                
        PWME_PWME0=1;        
        PWMPOL_PPOL0=1;        
        PWMPRCLK=0x00;                
        PWMDTY0=0x04;        
        PWMPER0=0x08;                                                        
}   

