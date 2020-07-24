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

## LAB4
I use the following code in [main.c](https://github.com/Paidalin/Lab-for-Embedded-Systems-Design/blob/master/prj4/Core/Src/main.c) to control the blinking of the three LEDs.
```
#include "main.h"
#include "cmsis_os.h"
#include "queue.h"
#include "semphr.h"
	...
int counter = 0;
int queue1_c = 0;
int queue2_c = 0;
MSGQUEUE_OBJ_t msg;
QueueHandle_t myQueue1;
QueueHandle_t myQueue2;
SemaphoreHandle_t myMutex;
	...
myQueue1 = xQueueCreate(10, sizeof(unsigned long));
myQueue2 = xQueueCreate(10, sizeof(unsigned long));
myMutex = xSemaphoreCreateMutex();
	...
void StartDefaultTask(void *argument)
{
  for(;;)
  {
	if (uxQueueSpacesAvailable(myQueue1) && uxQueueSpacesAvailable(myQueue2)) {
		counter = 0x1+counter;
		xQueueSend(myQueue1, &counter, 0);
	}
	osDelay(1000);
	if (uxQueueSpacesAvailable(myQueue1) && uxQueueSpacesAvailable(myQueue2)) {
		counter = 0x1+counter;
		xQueueSend(myQueue1, &counter, 0);
		xQueueSend(myQueue2, &counter, 0);
	}
	osDelay(1000);
  }
}

void StartTask02(void *argument)
{
  for(;;)
  {
	int i;
	xQueueReceive(myQueue1, &queue1_c, 10);
	for (i=0; i<queue1_c; i++) {
		xSemaphoreTake(myMutex, portMAX_DELAY);
		HAL_GPIO_TogglePin(GPIOB, GPIO_PIN_14);
		HAL_GPIO_TogglePin(GPIOB, GPIO_PIN_7);
		osDelay(300);
		HAL_GPIO_TogglePin(GPIOB, GPIO_PIN_14);
		HAL_GPIO_TogglePin(GPIOB, GPIO_PIN_7);
		osDelay(300);
		xSemaphoreGive(myMutex);
		osDelay(1);
	}
  }
}

void StartTask03(void *argument)
{
  for(;;)
  {
	int j;
	xQueueReceive(myQueue2, &queue2_c, 10);
	for (j=0; j<queue2_c; j++) {
		xSemaphoreTake(myMutex, portMAX_DELAY);
		HAL_GPIO_TogglePin(GPIOB, GPIO_PIN_14);
		HAL_GPIO_TogglePin(GPIOB, GPIO_PIN_0);
		osDelay(300);
		HAL_GPIO_TogglePin(GPIOB, GPIO_PIN_14);
		HAL_GPIO_TogglePin(GPIOB, GPIO_PIN_0);
		osDelay(300);
		xSemaphoreGive(myMutex);
		osDelay(1);
	}
  }
}
```
![GIF](https://github.com/Paidalin/Lab-for-Embedded-Systems-Design/blob/master/Displayed/Show_video_lab3.gif)  
The LD2 flashes twice in a row at the beginning (I click the RESET botton), and then LD1 and LD2 flash alternately. This phenomenon also proves the correctness of my code, because queue2 has no data after 600ms, so Task2 still uses LED, and then Task2 and Task3 alternately occupy LED (LD3).
|   | Queue1 | Queue2 |
| :------| ------: | :------: |
| 600ms | 1 | 0 |
| 1200ms | 2 | 2 |
