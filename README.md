## LAB2
I use the following code in [main.c](https://github.com/Paidalin/Lab-for-Embedded-Systems-Design/blob/master/prj2/Core/Src/main.c) to control the blinking of the two LEDs. 
```
void StartDefaultTask(void *argument)
{
  for(;;)
  {
	HAL_GPIO_TogglePin(GPIOB, GPIO_PIN_7);
    osDelay(300);
  }
}

void StartTask02(void *argument)
{
  for(;;)
  {
	HAL_GPIO_TogglePin(GPIOB, GPIO_PIN_14);
    osDelay(900);
  }
}
```
![GIF](https://github.com/Paidalin/Lab-for-Embedded-Systems-Design/blob/master/Displayed/Show_video_lab2.gif)
## LAB3
I use the following code in [main.c](https://github.com/Paidalin/Lab-for-Embedded-Systems-Design/blob/master/prj3/Core/Src/main.c) to control the blinking of the two LEDs.
```
#include "queue.h"
	...
QueueHandle_t myQueue;
	...
myQueue = xQueueCreate(10, sizeof(unsigned long));
	...
void StartDefaultTask(void *argument)
{
  for(;;)
  {
	msg.Buf[0] = 0x7;
	msg.Idx = 0;
//	osMessageQueuePut(myQueue01Handle, &msg, 0, NULL);
	xQueueSend(myQueue, &msg, 0);
	HAL_GPIO_TogglePin(GPIOB, GPIO_PIN_14);
	osDelay(100);
	HAL_GPIO_TogglePin(GPIOB, GPIO_PIN_14);
	osDelay(900);
  }
}

void StartTask02(void *argument)
{
  for(;;)
  {
//	status = osMessageQueueGet(myQueue01Handle, &msg, NULL, NULL);
	status = xQueueReceive(myQueue, &msg, 10);
	if(status == pdPASS) {
//		osMessageQueueReset(myQueue01Handle);
		HAL_GPIO_TogglePin(GPIOB, GPIO_PIN_7);
		osDelay(100);
		HAL_GPIO_TogglePin(GPIOB, GPIO_PIN_7);
	}
  }
}
```
Before and after changing the code, the flashing of the small light did not change, so the video was not replaced.
![GIF](https://github.com/Paidalin/Lab-for-Embedded-Systems-Design/blob/master/Displayed/Show_video_lab3.gif)  
As you can see, although the second task does not set a delay of one second flashing LD2, but because data will be received every second, his flashing interval is also one second.