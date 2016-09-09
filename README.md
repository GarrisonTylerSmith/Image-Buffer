# Image-Buffer
In this Image Buffer code I will try and create a project that allows me to read and write a PPm image (P3 or P6).

Hello , My name is Garrison Smith and I will be writing code pertaining to my image buffer class so far I started off with
#include <stdio.h>
#include <stdlib.h>

typedef struct{
    unsigned char red, green,blue;
}
PPMPixel;
typedef struct{
    int x, y;
    PPMPixel *data;
}
PPMImage;
