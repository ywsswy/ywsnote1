const struct Sensor slider =
               {
                  .halDefinition = fRO_COMPB_TA1_SW,
                  .numElements = 5,
                  .baseOffset = 0,
                  .cbpdBits = 0x001F, //BIT0+BIT1+BIT2+...BITE+BITF
				  .points = 100,
				  .sensorThreshold = 75,
                  // Pointer to elements
                  .arrayPtr[0] = &element0,  // point to first element
                  .arrayPtr[1] = &element1,
                  .arrayPtr[2] = &element2,
                  .arrayPtr[3] = &element3,
                  .arrayPtr[4] = &element4,
