/***************************************************************************//**
 * @brief   Writes a character from FONT6x8[] array to the LCD at (x,y).
 *
 *          (0,0) is the upper left corner of screen.
 * @param   x Horizontal coordinate (0 - 102)
 * @param   y Vertical coordinate (0 - 63)
 * @param   f Pointer to the character to be written to the LCD
 * @param   style The style of the text
 *                - NORMAL = 0 = dark letters with light background
 *                - INVERT = 1 = light letters with dark background
 * @return  None
 ******************************************************************************/
void Dogs102x6_charDrawXY(uint8_t x, uint8_t y, uint16_t f, uint8_t style)
{
    // Each Character consists of 6 Columns 8 pixels tall
    uint8_t b, row;
    uint16_t h;
    uint8_t desired_char[12];
    // make sure we won't be writing off the screen
    if (x >= 102)
    {
        x = 101;
    }
    if (y >= 64)
    {
        y = 63;
    }
    // handle characters not in our table
    if (f < 32 || f > 129)
    {
        // replace the invalid character with a '.'
        f = '.';
    }
    // subtract 32 because FONT6x8[0] is "space" which is ascii 32,
    // multiply by 6 because each character is 6 columns wide
    h = (f - 32) * 6;
    // Check if there is a remainder
    row = y / 8;
    if (style == DOGS102x6_DRAW_NORMAL)
    {
        for (b = 0; b < 6; b++)
        {
            desired_char[b] =
                (FONT6x8[h + b] >> (y % 8)) | dogs102x6Memory[2 + (row * 102) + x + b];
            desired_char[b + 6] =
                FONT6x8[h + b] << (8 - y % 8) | dogs102x6Memory[2 + ((row + 1) * 102) + x + b];
        }
    }
    else
    {
        for (b = 0; b < 6; b++)
        {
            desired_char[b] = (FONT6x8[h + b] ^ 0xFF) >> (y % 8);
            desired_char[b + 6] = (FONT6x8[h + b] ^ 0xFF) << (8 - y % 8);
        }
    }
    Dogs102x6_setAddress(row, x);
    // write first line of character
    Dogs102x6_writeData(desired_char, 6);
    Dogs102x6_setAddress(row + 1, x);
    // write second line of character
    Dogs102x6_writeData(desired_char + 6, 6);
}

