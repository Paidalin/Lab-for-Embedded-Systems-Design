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
