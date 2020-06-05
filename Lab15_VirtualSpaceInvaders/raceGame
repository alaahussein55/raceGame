#include "..//tm4c123gh6pm.h"
#include "Nokia5110.h"
#include "Random.h"
#include "TExaS.h"

void startscreen(void);
void gameoverscreen(void);
void startGame(void);
void defendermovement(void);
void attackermovement(void);
void PortF_Init(void);
void DisableInterrupts(void); // Disable interrupts
void EnableInterrupts(void);  // Enable interrupts
void Timer2_Init(unsigned long period);
void Delay100ms(unsigned long count); // time delay in 0.1 seconds
unsigned long TimerCount;
unsigned long Semaphore;


// *************************** Images ***************************


const unsigned char defender[] ={
0x42, 0x4D, 0xC6, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x76, 0x00, 0x00, 0x00, 0x28, 0x00, 0x00, 0x00, 0x10, 0x00, 0x00, 0x00, 0x0A, 0x00, 0x00, 0x00, 0x01, 0x00, 0x04, 0x00, 0x00, 0x00,
 0x00, 0x00, 0x50, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x80, 0x00, 0x00, 0x80,
 0x00, 0x00, 0x00, 0x80, 0x80, 0x00, 0x80, 0x00, 0x00, 0x00, 0x80, 0x00, 0x80, 0x00, 0x80, 0x80, 0x00, 0x00, 0x80, 0x80, 0x80, 0x00, 0xC0, 0xC0, 0xC0, 0x00, 0x00, 0x00, 0xFF, 0x00, 0x00, 0xFF,
 0x00, 0x00, 0x00, 0xFF, 0xFF, 0x00, 0xFF, 0x00, 0x00, 0x00, 0xFF, 0x00, 0xFF, 0x00, 0xFF, 0xFF, 0x00, 0x00, 0xFF, 0xFF, 0xFF, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x0F,
 0x00, 0x00, 0x00, 0x00, 0xF0, 0x00, 0x00, 0x00, 0xF0, 0x00, 0x00, 0x0F, 0x00, 0x00, 0x00, 0x0F, 0xFF, 0xFF, 0xFF, 0xFF, 0xF0, 0x00, 0x00, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0x00, 0x00, 0xFF,
 0xF0, 0xFF, 0xFF, 0x0F, 0xFF, 0x00, 0x00, 0xF0, 0xFF, 0xFF, 0xFF, 0xFF, 0x0F, 0x00, 0x00, 0xF0, 0x0F, 0x00, 0x00, 0xF0, 0x0F, 0x00, 0x00, 0x00, 0xF0, 0x00, 0x00, 0x0F, 0x00, 0x00, 0x00, 0x00,
 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0xFF
};

const unsigned char attacker[] ={
 0x42, 0x4D, 0xC6, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x76, 0x00, 0x00, 0x00, 0x28, 0x00, 0x00, 0x00, 0x10, 0x00, 0x00, 0x00, 0x0A, 0x00, 0x00, 0x00, 0x01, 0x00, 0x04, 0x00, 0x00, 0x00,
 0x00, 0x00, 0x50, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x80, 0x00, 0x00, 0x80,
 0x00, 0x00, 0x00, 0x80, 0x80, 0x00, 0x80, 0x00, 0x00, 0x00, 0x80, 0x00, 0x80, 0x00, 0x80, 0x80, 0x00, 0x00, 0x80, 0x80, 0x80, 0x00, 0xC0, 0xC0, 0xC0, 0x00, 0x00, 0x00, 0xFF, 0x00, 0x00, 0xFF,
 0x00, 0x00, 0x00, 0xFF, 0xFF, 0x00, 0xFF, 0x00, 0x00, 0x00, 0xFF, 0x00, 0xFF, 0x00, 0xFF, 0xFF, 0x00, 0x00, 0xFF, 0xFF, 0xFF, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x0F,
 0x00, 0x00, 0x00, 0x00, 0xF0, 0x00, 0x00, 0x00, 0xF0, 0x00, 0x00, 0x0F, 0x00, 0x00, 0x00, 0x0F, 0xFF, 0xFF, 0xFF, 0xFF, 0xF0, 0x00, 0x00, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0x00, 0x00, 0xFF,
 0xF0, 0xFF, 0xFF, 0x0F, 0xFF, 0x00, 0x00, 0xF0, 0xFF, 0xFF, 0xFF, 0xFF, 0x0F, 0x00, 0x00, 0xF0, 0x0F, 0x00, 0x00, 0xF0, 0x0F, 0x00, 0x00, 0x00, 0xF0, 0x00, 0x00, 0x0F, 0x00, 0x00, 0x00, 0x00,
 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0xFF

};



// *************************** Capture image dimensions out of BMP**********

#define defenderW        ((unsigned char)defender[18])
#define defenderH        ((unsigned char)defender[22])
#define attackerW        ((unsigned char)attacker[18])
#define attackerH        ((unsigned char)attacker[22])


// ***************************** Variables ************************ //

int defenderpx , defenderpy ;			  //defender position(x, y) 
int attackerY1, attackerx ,attackerMove, attackerMove1;		//attacker position and movement
int screen_width = 84, screenH = 47;						//screen diminssions
int tracks[3] = {42, 7, 57};										//tracks y positions
int f1 = 0, f2 = 0;									//flags
int level;

int main(void){
	
	// ******************** initializations ***************************** //
	PortF_Init();
	defenderpy=48; //first position y for defender
defenderpx = 42;	//first position x for defender	
	attackerMove = 25-attackerH; 
	attackerMove1 = 25-attackerH;
	attackerx = rand()%3;
	level = 0; //number of passes
	
  TExaS_Init(SSI0_Real_Nokia5110_Scope);  // set system clock to 80 MHz
  Random_Init(1);
  Nokia5110_Init();
  Nokia5110_ClearBuffer();
	Nokia5110_DisplayBuffer();      // draw buffer
	
	startscreen();//welcome screen
  
	startGame(); //beginning of the game 
	
	// ***************************** Some delay befor begining ************************ //
	Nokia5110_ClearBuffer();
	Nokia5110_PrintBMP(defenderpx, defenderpy, defender, 0);
	Nokia5110_DisplayBuffer();
	Delay100ms(300);
	
	// ***************************** Game loop ************************ //
	while(1){
		Nokia5110_ClearBuffer();		
		
								
		defendermovement(); //defender function movement
		
		if(attackerMove !=48){  //attacker movement
			Nokia5110_PrintBMP( tracks[attackerx],attackerMove, attacker, 0);
			attackerMove++;
		}
		
		if(attackerMove == 48){
			
			if(attackerMove1 == 48){
				level++;					//number of levels
				attackerx = rand()%3;
				attackerMove1 = 25-attackerH;
			}
			Nokia5110_PrintBMP(tracks[attackerx],attackerMove1,  attacker, 0);
		}
		
		if(attackerMove1 >= defenderpy-defenderW && tracks[attackerx] == defenderpx){
			break;
		}
		attackerMove1++;
				
								//***************** Screen displaying ********************//
		Nokia5110_DisplayBuffer();
		Delay100ms(1);
	}
	
	Delay100ms(50);				//Delay befor gameover screen

						 
 gameoverscreen(); //function of game over screen
}
//end of main
void startscreen(void){
Nokia5110_Clear();
	Nokia5110_SetCursor(3, 0);
  Nokia5110_OutString("welcom");
  Nokia5110_SetCursor(1, 2);
  Nokia5110_OutString("Racing game");
  Nokia5110_SetCursor(0, 4);
  Nokia5110_OutString("to start");
  Nokia5110_SetCursor(1, 5);
  Nokia5110_OutString("Press SW1");

}



void startGame(void){
	
	while(1){
		if(((GPIO_PORTF_DATA_R&(1<<4)) == 0))
			break;
  }}

	void defendermovement(void){
	
	if(!(((GPIO_PORTF_DATA_R&(1<<4)) == 0) && ((GPIO_PORTF_DATA_R&0x1) == 0))){
			if(((GPIO_PORTF_DATA_R&(1<<4)) == 0)){
				if((defenderpx > 7) && (f1 == 0)){
					defenderpx -= 35;
					f1 = 1;
				}
			}
			else{
				f1 = 0;
			}

			if(((GPIO_PORTF_DATA_R&0x1) == 0)){
				if((defenderpx < 70) && (f2 == 0)){
					defenderpx += 15;
					f2 = 1;
				}
			}
			else{
				f2 = 0;
			}
		}
		
		Nokia5110_PrintBMP(defenderpx, defenderpy, defender, 0);
	
	}
	
	void gameoverscreen(void){
	 Nokia5110_Clear();
  Nokia5110_SetCursor(1, 0);
  Nokia5110_OutString("GAME OVER");
  Nokia5110_SetCursor(1, 2);
  Nokia5110_OutString("Nice try");
	Nokia5110_SetCursor(1, 4);
  Nokia5110_OutString("your score is");
	Nokia5110_SetCursor(3, 5);
  Nokia5110_OutUDec(level);

	}
void PortF_Init(void){ volatile unsigned long delay;
  SYSCTL_RCGC2_R |= 0x00000020;     // 1) F clock
  delay = SYSCTL_RCGC2_R;           // delay   
  GPIO_PORTF_LOCK_R = 0x4C4F434B;   // 2) unlock PortF PF0  
  GPIO_PORTF_CR_R = 0x1F;           // allow changes to PF4-0       
  GPIO_PORTF_AMSEL_R = 0x00;        // 3) disable analog function
  GPIO_PORTF_PCTL_R = 0x00000000;   // 4) GPIO clear bit PCTL  
  GPIO_PORTF_DIR_R = 0x0E;          // 5) PF4,PF0 input, PF3,PF2,PF1 output   
  GPIO_PORTF_AFSEL_R = 0x00;        // 6) no alternate function
  GPIO_PORTF_PUR_R = 0x11;          // enable pullup resistors on PF4,PF0       
  GPIO_PORTF_DEN_R = 0x1F;          // 7) enable digital pins PF4-PF0        
}


// You can use this timer only if you learn how it works
void Timer2_Init(unsigned long period){ 
  unsigned long volatile delay;
  SYSCTL_RCGCTIMER_R |= 0x04;   // 0) activate timer2
  delay = SYSCTL_RCGCTIMER_R;
  TimerCount = 0;
  Semaphore = 0;
  TIMER2_CTL_R = 0x00000000;    // 1) disable timer2A during setup
  TIMER2_CFG_R = 0x00000000;    // 2) configure for 32-bit mode
  TIMER2_TAMR_R = 0x00000002;   // 3) configure for periodic mode, default down-count settings
  TIMER2_TAILR_R = period-1;    // 4) reload value
  TIMER2_TAPR_R = 0;            // 5) bus clock resolution
  TIMER2_ICR_R = 0x00000001;    // 6) clear timer2A timeout flag
  TIMER2_IMR_R = 0x00000001;    // 7) arm timeout interrupt
  NVIC_PRI5_R = (NVIC_PRI5_R&0x00FFFFFF)|0x80000000; // 8) priority 4
// interrupts enabled in the main program after all devices initialized
// vector number 39, interrupt number 23
  NVIC_EN0_R = 1<<23;           // 9) enable IRQ 23 in NVIC
  TIMER2_CTL_R = 0x00000001;    // 10) enable timer2A
}
void Timer2A_Handler(void){ 
  TIMER2_ICR_R = 0x00000001;   // acknowledge timer2A timeout
  TimerCount++;
  Semaphore = 1; // trigger
}
void Delay100ms(unsigned long count){unsigned long volatile time;
  while(count>0){
    time = 727240/80;  // 0.1sec at 80 MHz
    while(time){
	  	time--;
    }
    count--;
  }
}