# Cat-Printer

This is an example program to print with a portable BLE thermal printer using a WeMos D1 mini ESP32.

![cat](https://github.com/user-attachments/assets/c2598a93-1fd4-4644-b8bc-6e6060ee53a2) <br>
The [thermal printer](https://aliexpress.com/item/1005007505741207.html) I used.

# Libraries
This example is using the [Thermal_Printer](https://github.com/bitbank2/Thermal_Printer.git) library. <br>
The ID on my printer did not match with any of the ones preset in the library, so I had to add it manually to the [`szPrinterIDs[]`](https://github.com/bitbank2/Thermal_Printer/blob/84f25aff59c4a0e998bb02f8c25b9d2e517317c8/src/Thermal_Printer.cpp#L68).

# Bitmaps
In order to print any arbitrary image you need to convert it to a bitmap with 1 bit per pixel.

**Original:** <br>
![pikachu](https://github.com/user-attachments/assets/e8245cae-33d2-442b-b319-a6375aae2941)

Using [GIMP](https://www.gimp.org/) I was able to convert the picture into a bitmap: <br>
***Bitmap Creation:** Image > Mode > Indexed > Use black and white (1-bit) palette* <br>
***Bitmap Export:** File > Export > \<name>.bmp*

> [!TIP]
> Play around with the `Color-dithering` setting to get the best conversion.

**Bitmap:** <br>
![bitmap](https://github.com/user-attachments/assets/2bec18af-15cb-488b-acde-4b97624dae67)

In order to use this bitmap in our program it needs to be converted, once more, into a C byte array. <br>
To get the actual byte information out of a bitmap file a hex editor can be used, in my case I used [Okteta](https://apps.kde.org/okteta/).

Using [Okteta](https://apps.kde.org/okteta/) to open the BMP file and export as C array: <br>
***Open BMP**: File > Open > \<name>.bmp* <br>
***C Array Export**: File > Export > C Array > Data Type: unsigned char*

**Byte Array:** <br>
```c
static uint8_t bitmap[2110] =
{
  0x42, 0x4d, 0x3e, 0x08, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x3e, 0x00, 0x00, 0x00, 0x28, 0x00, 0x00, 0x00, 0x80, 0x00, 0x00, 0x00, 0x80, 0x00, 0x00, 0x00, 0x01, 0x00, 0x01, 0x00, 0x00, 0x00,
  ...
};
```

> [!IMPORTANT]  
> 1. Make sure you change the type declaration from `unsigned char` to `uint8_t` in order for it to work with the library!
> 2. Change the [`width` and `height` constants](https://github.com/moeux/cat-printer/blob/a7645a893e16617e189b8df2ccf59ed3130f3505/src/main.cpp#L4) to the width and the height of the image you used!

# Troubleshooting

**No printer found**

The library connects successfully only to printers which IDs have been registered in [`szPrinterIDs[]`](https://github.com/bitbank2/Thermal_Printer/blob/84f25aff59c4a0e998bb02f8c25b9d2e517317c8/src/Thermal_Printer.cpp#L68). <br>
Make sure your printer's ID is registered.

If you'd like to find out the ID of your printer, enable the [`DEBUG_OUTPUT`](https://github.com/bitbank2/Thermal_Printer/blob/84f25aff59c4a0e998bb02f8c25b9d2e517317c8/src/Thermal_Printer.cpp#L23) to see all clients (and their IDs) that were found during the Bluetooth scan.
