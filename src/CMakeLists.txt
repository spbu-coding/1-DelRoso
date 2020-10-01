#include <math.h>
#include <stdlib.h>
#include <stdio.h>
#include <string.h>

#define MAX_RESULT_STRING_LENGTH 32

#define PI  3.14159

#pragma warning(disable : 4996)
#pragma warning(disable : 6031)

char str[6][25];
double a = 0, b = PI;
double l_bord, r_bord;

double rectangle_method(int n)
{
	double result = 0, sector;
	sector = (r_bord - l_bord) / n;

	for (int i = 0; i < n; i++)
	{
		result += sin(l_bord + sector * (i + 0.5));
	}

	result *= sector;
	return result;
}

double pryam(int n) 
{
	double x, h, integral = 0;
	h = (b - a) / n;
	x = a;
	for (int i = 0; i < n; i++) 
	{
		integral += sin(x + h / 2) * h;
		x += h;
	}
	return integral;
}

double simps(int n) 
{
	double x, h, integral;
	h = (b - a) / 2 / n;
	integral = sin(a) + sin(b);
	x = a + h;
	for (int i = 1; i < 2 * n; i++)
	{
		if (i % 2 == 0) integral += 2 * sin(x);
		else integral += 4 * sin(x);
		x += h;
	}
	integral *= h / 3;
	return integral;
}

void entering_numbers() 
{
	int borderA = 0, borderB = 0, borderAll = 0;
	while (!borderAll)
	{
		while (!borderA) 
		{
			printf("Enter interval's left border: ");
			scanf("%lf", &l_bord);
			if (l_bord < 0) 
			{
				printf("Error:Left border must be more than or equal to 0 \n");
				continue;
			}
			else if (l_bord > PI) 
			{
				printf("Error: Left border must be less than PI \n");
				continue;
			}
			borderA = 1;
		}
		while (!borderB) 
		{
			printf("Enter interval's right border:");
			scanf("%lf", &r_bord);
			if (r_bord < 0)
			{
				printf("Error: Right border must be more than 0 \n");
				continue;
			}
			else if (r_bord > PI)
			{
				printf("Error: Right border must be less than PI \n");
				continue;
			}
			if (l_bord > r_bord) 
			{
				printf("Error: Right border must be more than left \n");
				continue;
			}
			borderB = 1;
		}
		borderAll = 1;
	}
}

void console(int array[6]) 
{
	char f_per[15];
	for (int i = 0; i < 6; i++) 
	{
		int j = 0;
		double pr, simp;
		pr = pryam(array[i]);
		simp = simps(array[i]);
		sprintf(f_per, "%d", array[i]);
		strcat(str[i], f_per);
		strcat(str[i], " ");
		sprintf(f_per, "%2.5f", pr);
		strcat(str[i], f_per);
		strcat(str[i], " ");
		sprintf(f_per, "%2.5f", simp);
		strcat(str[i], f_per);
	}
}

int main()
{
	int array[] = { 5, 10, 20, 100, 500, 1000 };
	entering_numbers();
	console(array);
	for (int i = 0; i < 6; i++) 
	{
		printf("%s\n", str[i]);
	}
	return 0;
}
