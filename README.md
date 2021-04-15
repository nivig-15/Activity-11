# Activity-11
#include<stdio.h>

typedef struct
{
	char sname[10];
	int marks[10];
      	char grade;
      	float avg;
}student;

typedef struct
{
	 char course[10];
	 int sno;
     	 float weights[10];
     	 int no_weights;
      	 student std[10];
}gradebook;

int input_n()
{
    	int n;
	printf("Enter the number of gradebooks:");
	scanf("%d",&n);
	return n;
}

student input_student(int no_weights)
{
	student s;
	printf("\nEnter the student name: ");
	scanf("%s", s.sname);
	printf("Enter the marks of the student: ");
	for(int i=0;i<no_weights;i++)
	{
		scanf("%d",&s.marks[i]);
	}
	return s;
}

gradebook input_gradebook()
{
	gradebook gb;
	printf("\nEnter the course name: ");
	scanf("%s", gb.course);
	printf("Enter no of students and no of weights: ");
	scanf("%d%d", &gb.sno, &gb.no_weights);
	printf("Enter value of weights: ");
	for(int i=0;i<gb.no_weights;i++)
	{
		scanf("%f",&gb.weights[i]);
	}
	for(int i=0;i<gb.sno;i++)
	{
		gb.std[i] = input_student(gb.no_weights);
	}
	return gb;
}

void input_n_gradebook(int n, gradebook gb[n])
{
	for(int i=0;i<n;i++)
	{
		gb[i] = input_gradebook();
	}
}

char compute_grade(float marks)
{
	char grade;
	if(marks>=0 && marks<60)
		grade = 'F';
	else if(marks>=60 && marks<70)
		grade = 'D';
	else if(marks>=70 && marks<80)
		grade = 'C';
      	else if(marks>=80 && marks<90)
		grade = 'B';
      	else if(marks>=90 && marks<=100)
		grade = 'A';
	return grade;
}

void compute_students(student *std, gradebook gb)
{
	float total_weights,total_score;
	for(int i=0;i<gb.no_weights;i++)
	{
		total_weights += gb.weights[i];
	}
	for(int i=0;i<gb.no_weights;i++)
	{
		total_score += std->marks[i]*gb.weights[i];
	}
	std->avg = total_score/total_weights;
	std->grade = compute_grade(std->avg);
}

void compute_gradebook(gradebook *gb)
{
    for(int i=0;i<gb->sno;i++)
    {
        compute_students(&gb->std[i],*gb);
    }
}

void compute_n_gradebook(int n, gradebook gb[n])
{
	for(int i=0;i<n;i++)
	{
		compute_gradebook(&gb[i]);
	}
}

void output_gradebook(gradebook gb)
{
	for(int i=0;i<gb.sno;i++)
	{
		printf("\n%s \t %.2f %c",gb.std[i].sname,gb.std[i].avg,gb.std[i].grade);
	}
}

void output_n_gradebook(int n, gradebook gb[n])
{
	for(int i=0;i<n;i++)
	{
	   	 printf("\n%s",gb[i].course);
		 output_gradebook(gb[i]);
	}
}

int main()
{
	int n;
	n = input_n();
	gradebook gb[n];
	input_n_gradebook(n,gb);
	compute_n_gradebook(n,gb);
	output_n_gradebook(n,gb);
	return 0;
}
