#include <stdio.h> 
#include <math.h> 
int main() { 
	double y = 0.0;
	double x = 0.0;
	for (y = 1.3; y >= -1.1; y -= 0.1){
		for (x = -1.2; x <= 1.2; x += 0.05){
			if (pow((x*x + y*y - 1.0), 3) - x*x*y*y*y <= 0.0){
				printf("*");
			}
			else{
				printf(" ");
			}
		}
		printf("\n");
	}
	return 0; 
}

