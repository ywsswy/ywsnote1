#ifndef HAL_DOGS102X6_H
#define HAL_DOGS102X6_H
#include <stdint.h>
// Screen size
#define DOGS102x6_X_SIZE   102         // Display Size in dots: X-Axis
#define DOGS102x6_Y_SIZE    64         // Display Size in dots: Y-Axis
// Screen printing styles
#define DOGS102x6_DRAW_NORMAL   0x00   // Display dark pixels on a light background
#define DOGS102x6_DRAW_INVERT   0x01   // Display light pixels on a dark background
extern uint8_t dogs102x6Memory[];      // Provide direct access to the frame buffer
extern void Dogs102x6_init(void);
//extern void Dogs102x6_backlightInit(void);
extern void Dogs102x6_disable(void);
extern void Dogs102x6_writeCommand(uint8_t* sCmd, uint8_t i);
extern void Dogs102x6_writeData(uint8_t* sData, uint8_t i);
extern void Dogs102x6_setAddress(uint8_t pa, uint8_t ca);
extern uint8_t Dogs102x6_getContrast(void);
extern void Dogs102x6_setContrast(uint8_t newContrast);
extern void Dogs102x6_setInverseDisplay(void);
extern void Dogs102x6_clearInverseDisplay(void);
extern void Dogs102x6_scrollLine(uint8_t lines);
extern void Dogs102x6_setAllPixelsOn(void);
extern void Dogs102x6_clearAllPixelsOn(void);
extern void Dogs102x6_clearScreen(void);
extern void Dogs102x6_charDraw(uint8_t row, uint8_t col, uint16_t f, uint8_t style);
//row:0~7	col:0~102
extern void Dogs102x6_stringDraw(uint8_t row, uint8_t col, char *word, uint8_t style);
extern void Dogs102x6_pixelDraw(uint8_t x, uint8_t y, uint8_t style);
extern void Dogs102x6_horizontalLineDraw(uint8_t x1, uint8_t x2, uint8_t y, uint8_t style);
extern void Dogs102x6_verticalLineDraw(uint8_t y1, uint8_t y2, uint8_t x, uint8_t style);
extern void Dogs102x6_lineDraw(uint8_t x1, uint8_t y1, uint8_t x2, uint8_t y2, uint8_t style);
extern void Dogs102x6_circleDraw(uint8_t x, uint8_t y, uint8_t radius, uint8_t style);
extern void Dogs102x6_imageDraw(const uint8_t IMAGE[], uint8_t row, uint8_t col);
extern void Dogs102x6_clearImage(uint8_t height, uint8_t width, uint8_t row, uint8_t col);
#endif /* HAL_DOGS102x6_H */

