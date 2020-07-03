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
void StartDefaultTask(void *argument)
{
  for(;;)
  {
	msg.Buf[0] = 0x7;
	msg.Idx = 0;
	osMessageQueuePut(myQueue01Handle, &msg, 0, NULL);
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
	status = osMessageQueueGet(myQueue01Handle, &msg, NULL, NULL);
	if(status == osOK) {
		osMessageQueueReset(myQueue01Handle);
		HAL_GPIO_TogglePin(GPIOB, GPIO_PIN_7);
		osDelay(100);
		HAL_GPIO_TogglePin(GPIOB, GPIO_PIN_7);
	}
  }
}
```
![GIF](https://github.com/Paidalin/Lab-for-Embedded-Systems-Design/blob/master/Displayed/Show_video_lab3.gif)