#define contrastSetpointAddress     0x1880
void main(){
	uint8_t contrast = *((unsigned char *)contrastSetpointAddress);
	//若当前FLASH中无对比度值，则将对比度值设为11
	if (contrast == 0xFF)             
        	contrast = 11;
   	Dogs102x6_setContrast(contrast);
	//WriteFlashSettings(contrast, contrastSetpointAddress); //将新对比度值写入FLASH地址   保存设置
}
void WriteFlashSettings(uint16_t Data, uint16_t Address)
{
    uint16_t * Flash_ptr;                   // Initialize Flash pointer
    uint16_t Flash_Contents[16];            // Store Contents of Flash before Programming
    uint8_t i;
    Flash_ptr = (uint16_t *)(0x1880);       // Info C
    for (i = 0; i < 16; i++)                // We know that we will only use > 8 variables in Info C
    {
        Flash_Contents[i] = *Flash_ptr;     // Read a word from flash
        if (Flash_ptr == (uint16_t *)Address)
            Flash_Contents[i] = Data;
        *Flash_ptr++;
    }
    Flash_ptr = (uint16_t *)(0x1880);       // Info C
    FCTL3 = FWKEY;                          // Clear Lock bit
    FCTL1 = FWKEY + ERASE;                  // Set Erase bit
    *Flash_ptr = 0;                         // Dummy write to erase Flash seg
    FCTL1 = FWKEY + WRT;                    // Set WRT bit for write operation
    for (i = 0; i < 16; i++)
    {
        *Flash_ptr++ = Flash_Contents[i];   // Write a word to flash
    }
    FCTL1 = FWKEY;                          // Clear WRT bit
    FCTL3 = FWKEY + LOCK;                   // Set LOCK bit
}
#define brightnessSetpointAddress   0x1882
	//WriteFlashSettings(brightness, brightnessSetpointAddress); //将新背光值写入FLASH地址，保存设置

