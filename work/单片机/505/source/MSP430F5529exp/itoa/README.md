#include <stdint.h>
#include "msp430.h"
#include "HAL_PMM.h"
#include "HAL_UCS.h"
#include "HAL_Board.h"
#include "HAL_Buttons.h"
#include "HAL_Dogs102x6.h"
#include "HAL_Menu.h"
#include "HAL_Wheel.h"
#include "Clock.h"
#include "LPM.h"
#include "Random.h"
#include "PMM.h"
#include "Demo_Cube.h"
#include "CTS_Layer.h"
#include "stdlib.h"
#include "lab2.h"
static const char *const capMenuText[] = {
    "==LAB2:Cap App===",
    "1. CapLED ",
    "2. CapDemo ",
    "3. Simon",
};
char *itoa(int, char *, int);
