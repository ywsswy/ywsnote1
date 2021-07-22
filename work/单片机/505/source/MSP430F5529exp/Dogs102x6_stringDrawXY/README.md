/***************************************************************************//**
 * @brief   Writes a String to the LCD at (x,y).
 *
 *          (0,0) is the upper left corner of screen.
 * @param   x Horizontal coordinate
 * @param   y Vertical coordinate
 * @param   word[] Pointer to the String to be written to the LCD
 * @param   style The style of the text
 *                - NORMAL = 0 = dark letters with light background
 *                - INVERT = 1 = light letters with dark background
 * @return  None
 ******************************************************************************/
void Dogs102x6_stringDrawXY(uint8_t x, uint8_t y, char *word, uint8_t style)
{
    /* Each Character consists of 6 Columns on 1 Page
     * Each Page presents 8 pixels vertically (top = MSB)*/
    uint8_t a = 0;
    while (word[a] != 0)
    {
        // Draw a character
        Dogs102x6_charDrawXY(x, y, word[a], style);
        // Update location
        x += 6;
        // Text wrapping
        if (x >= 102)
        {
            x = 0;
            if (y + 8 < 64)
                y += 8;
            else
                y = 0;
        }
        a++;
    }
}

