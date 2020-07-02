
# AT51-curriculum-design
> This is a MCU Course Design Project base on Intel-at89c51
+ 南方某工业大学课程设计题目，设计出的第一代作品，现完成第三代，将第一代作品开源。
+ 实现的效果视频挂在哔哩哔哩弹幕网，有需要可以前往观看，顺便一键三连[doge]： [点击跳转](https://www.bilibili.com/video/BV1P7411T7ST)。
```c
/* Main.c file generated by New Project wizard
 *
 * Created: 2020年2月20日
 * Processor: AT89C52
 * Compiler:  Keil for 8051
 */

#include <reg51.h>
#include <stdio.h>

#define duanxuan P0        //段选接口，P1的所有接口都用
//#define weixuan P2      //位选接口

sbit N1=P2^0;
sbit N2=P2^1;
sbit N3=P2^2;
sbit N4=P2^3;
sbit led_green1=P1^0;
sbit led_yellow1=P1^1;
sbit led_red1=P1^2;
sbit led_green2=P1^3;
sbit led_yellow2=P1^4;
sbit led_red2=P1^5;

unsigned char code wxcode[4]={0xfe,0xfd,0xfb,0xf7};
unsigned char code dxcode[10]={0xc0,0xf9,0xa4,0xb0,0x99,0x92,0x82,0xf8,0x80,0x90};

/********************************************
                  定义函数区
********************************************/
void delayms(unsigned int a);
void DigDisplay(int num1,int num2);
void EW_LED_Time();
void NS_LED_Time();


void delayms(unsigned int a)
{
	unsigned int y;

	for(a;a>0;a--)
	 for(y=110;y>0;y--)
	       ;
}

void DigDisplay(int num1,int num2)
{
   unsigned char i=0;
   //unsigned int j=0;
   int gewei1,shiwei1;
   int gewei2,shiwei2;
   
   gewei1=num1%10;//东西方向
   shiwei1=num1/10%10;
   gewei2=num2%10;//南北方向
   shiwei2=num2/10%10;
/*   
   for(i=0;i<4;i++)
   {
      weixuan=wxcode[i];
      duanxuan=0xff;
      if(i==1) {duanxuan=dxcode[shiwei1];delayms(10);duanxuan=0xff;delayms(2);}
      if(i==0) {duanxuan=dxcode[gewei1];delayms(10);duanxuan=0xff;delayms(2);}
      if(i==3) {duanxuan=dxcode[shiwei2];delayms(10);duanxuan=0xff;delayms(2);}
      if(i==2) {duanxuan=dxcode[gewei2];delayms(10);duanxuan=0xff;delayms(2);}
      
      
      //duanxuan=0xff;
   }
*/
   N1=0;N2=0;N3=0;N4=0;//定义端口电平，如果没有定义会导致数字串位的现象
   N1=1;duanxuan=dxcode[shiwei1];delayms(10);duanxuan=0xff;N1=0;
   N2=1;duanxuan=dxcode[gewei1];delayms(10);duanxuan=0xff;N2=0;
   N3=1;duanxuan=dxcode[shiwei2];delayms(10);duanxuan=0xff;N3=0;
   N4=1;duanxuan=dxcode[gewei2];delayms(10);duanxuan=0xff;N4=0;
   
}

void NS_LED_Time()//南北方向的LED配合倒计时
{
   
}

void EW_LED_Time()//东西方向的LED配合倒计时
{
   //keil中定义变量一定要放在函数的最前面，否则内存分配会出现问题
   unsigned int greentime1=12,yellowtime1=3,redtime1=5;//定义南北方向每个灯的时间
   unsigned int greentime2=3,yellowtime2=2,redtime2=15;//定义东西方向每个灯的时间
   unsigned int j=0;//定义一个时间常数衡量1秒大概时间
   
   led_green1=0;led_yellow1=0;led_red1=0;//东西每个灯初始状态为关闭
   led_green2=0;led_yellow2=0;led_red2=0;//南北每个灯初始状态为关闭
   
   led_green1=1;led_red2=1;
   while(greentime1!=0)
   {
      j=23;//时间常数为23是1秒大概时间
      while(--j)
      {
	 DigDisplay(greentime1,redtime2);
      }
      greentime1--;redtime2--;
   }
   led_green1=0;
   
   led_yellow1=1;
   while(yellowtime1!=0)
   {
      j=23;//定义一个时间常数衡量1秒大概时间
      while(--j)
      {
	 DigDisplay(yellowtime1,redtime2);
      }
      yellowtime1--;redtime2--;
      led_yellow1=~led_yellow1;//对黄灯状态取反
   }
   led_yellow1=0;led_red2=0;
   
   led_red1=1;led_green2=1;
   while(greentime2!=0)
   {
      j=23;//定义一个时间常数衡量1秒大概时间
      while(--j)
      {
	 DigDisplay(redtime1,greentime2);
      }
      redtime1--;greentime2--;
   }
   led_green2=0;
   
   led_yellow2=1;
   while(yellowtime2!=0)
   {
      j=23;//定义一个时间常数衡量1秒大概时间
      while(--j)
      {
	 DigDisplay(redtime1,yellowtime2);
      }
      redtime1--;yellowtime2--;
      led_yellow2=~led_yellow2;//对黄灯状态取反
   }
   led_yellow2=0;led_red1=0;
   
   
}




void main(void)
{
   //int num1=60,num2=10;
   //unsigned int j=0;
 
   while(1)
   {
      EW_LED_Time();
   }
}
```
