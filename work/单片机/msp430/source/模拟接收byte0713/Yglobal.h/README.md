#ifndef YGLOBALVAR_H_
#define YGLOBALVAR_H_
#include <stdint.h>
extern int8_t Ytotalitems;
extern int8_t Ynowfirstitem;
extern int8_t Yonepageitems;
extern int8_t Yadcmodel;//0:自带ADC滚轮，1：外设ADC
extern int8_t Ys2model;//0 send 1 interupt
extern uint16_t Ypositiondata;
extern uint16_t Ypositionolddata;
extern volatile uint16_t Ybuttonspressed;
#endif

