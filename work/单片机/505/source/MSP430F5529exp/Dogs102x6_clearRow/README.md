/***************************************************************************//**
 * @brief   Clears one row/page (in memory as well).
 *
 *          Row 0 is at the top of the screen.
 * @param   row The row to be cleared (0~7)
 * @return  None
 ******************************************************************************/
void Dogs102x6_clearRow(uint8_t row)
{
    uint8_t cmd[] = {0};
    uint8_t a = 0;
    // Check row boundary
    if (row > 7)
    {
        row = 7;
    }
    Dogs102x6_setAddress(row, 0);
    for (a = 0; a < 102; a++)
    {
        Dogs102x6_writeData(cmd, 1);
        dogs102x6Memory[2 + (row * 102) + a] = 0x00;
    }
}
