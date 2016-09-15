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
So the next part of this code is checking the errors for the user when they try and write or edit the image 
So this is seems like the easiest way to check all the errors hwoever I do not know if I am checking everything though 
// check the format of the image
	if(buff[0] != 'P' || buff[1] != '6' || buff[1] != '3'){
		fprintf(stderr, "Invalid image format (MUST BE P6 or P3)'%s'\n" );
		exit(1);
	}
	// alloc memory from image
	img = (PPMImage *)malloc(sizeof(PPMImage));
	if(!img){
		fprintf(stderr, "Unable to allocate memory '%s'\n" );
		exit(1);

	}
	// next we need to check for comments
	c = getc(fp);
	while(c== '#'){
	while(getc(fp) != '\n');
		c = getc(fp);
	}
	unget(c,fp);
	// here we will read the image size and information about it
	if(fscanf(fp, "%d %d", &img->x, &img->y) != 2){
		fprintf(stderr, "Invalid image size (error loading '%s')\n", filename);
		exit(1);
	}
	// read rgb component
	if (fscanf(fp, "%d", &rgb_comp_color)!= 1){
		fprintf(stderr, "Invalid rgb component(error Loading '%s')\n", filename);
		exit(1);
	}
	// check rgb component depth 
	if(rgb_comp_color != RGB_COMPONENT_COLOR){
		fprintf(stderr, "'%s'does not have 8-bits components\n", filename);
		exit(1);
	}
	while(fgetc(fp) != '\n');
	// memory allocation for pixal data
	img->data = (PPMPixel*)malloc(img->x * img->y * sizeof(PPMPixel));

	if(!img){
		fprintf(stderr, "Unable to allocate memory\n" );
		exit(1);
	}

	//read pixal data from file
	if( fread(img->data, 3 * img-x, img->y, fp)!= img->y){
		fprintf(stderr, "Error loading image '%s'\n", filename);
		exit(1);
	}
	fclose(fp);
	return img;
}
void writePPM(const char *filename, PPMImage *img){
	FILE *fp;
	// open file for output
	fp = fopen(filename, "wb");
	if(!fp){
		fprintf(stderr, "Unable to open file '%s'\n", filename);
		exit(1);
	}
	// write the header file
	// image format
	fprintf(fp,"P6\n");
	fprintf(fp, "P3\n");
	// comments
	fprintf(fp, "# Created by %s\n", CREATOR);
	// image size
	fprintf(fp, "%d %d\n", img->x,img->y);
	// rgb component depth
	fprintf(fp, "%d\n", RGB_COMPONENT_COLOR);

	// pixal data
	fwrite(img->data, 3 * img->x, img->y, fp);
	fclose(fp);
}
void changeColorPPM(PPMImage *img){
	if(img){
		for(int i=0;i<img->x*img->y;i++){
			img->data[i].red=RGB_COMPONENT_COLOR-img->data[i].red;
			img->data[i].green=RGB_COMPONENT_COLOR-img->data[i].green;
			img->data[i].blue=RGB_COMPONENT_COLOR-img->data[i].blue;

		}
	}
}
int main(int argc, char *argv[]){
	printf(argv[0]);
	PPMImage *image;
	image = readPPM("can_bottom.ppm");
	changeColorPPM(image);
	writePPM("can_bottom2.ppm",image);
	printf("Press any key...");
	getchar();
}
 This new added code was implementiung the write methods and the changing of the colors method 
 I also added a main method which testes the code and its ablitiy to run . So far I am struggling with it but I will continuing to test
