File Handling and Doubly Linked List (CASE STUDY - PROGRAMMING 1)

#include <stdio.h>

#include <stdlib.h>

#include <string.h>

#include <process.h>

//structure

struct employee {

    char employee_num[10];
    
    char employee_name[15];
    
    char stats_code;
    
    int hr;           
    
    float deductions;
    
};

// Function to compute overtime pay

float compute_overtime(int hr, float basic_rate) {  

    int hrs_overtime = 0;   
    
    if (hr > 160) {
    
        hrs_overtime = hr - 160;
        
    }
    
    return hrs_overtime * (1.5 * basic_rate);
    
}

// Function to input employee details

void employee_details(struct employee* records) {

    printf("\n");
    
    printf("Enter Employee Number: ");
    
    scanf("%s", records->employee_num);
    
    printf("Enter Employee Name: ");
    
    scanf(" %[^\n]", records->employee_name);
    

// Using while(1)  loop that is an infinite loop until it encounters the break

    while (1) {
    
        printf("Enter Employee's Status Code (R=Regular, C=Casual): ");
        
        scanf(" %c", &records->stats_code);
        
        getchar(); // Clear newline character from the input buffer
        

// To check if the entered status is either R for Regular and C for Casual

        if (records->stats_code == 'R' || records->stats_code == 'C') {
        
            break;
            
        } else {
        
            printf("Invalid Employee Type!\n\n");
            
        }
        
    }
    

// To enter the no. of hours worked and the deduction of employees which then stored in ‘record’ structure

    printf("Enter Hours Worked: ");
    
    scanf("%d", &records->hr);   
    
    printf("Enter Deductions: ");
    
    scanf("%f", &records->deductions);   
    
}

// Function to compute the net pay

float compute_netpay(float basic_salary, float pay_overtime, float deductions) {

    return basic_salary + pay_overtime - deductions;
    
}

// Function to print the employee details/data entered by the user in text file format

void print_employee_details(FILE* fp, struct employee* records, float basic_salary, float pay_overtime, float net_pay) {

    char stats_code[20]; // character array with a size of 20
    

// To check the status code of the employees (either R or C) in the ‘record’ structure

    if (records->stats_code == 'R')
    
        strcpy(stats_code, "Regular"); // to print ‘Regular’ if R is entered
        
    else if (records->stats_code == 'C')
    
        strcpy(stats_code, "Casual"); // to print ‘Casual’ if C is entered
        
    else
    
        strcpy(stats_code, "Unknown"); // to print ‘Unknown’ if neither R or C is entered
      

// To record the entered employee details into a text file format      

		fprintf(fp, "| %-20s | %-18s | %-10s | %-16d | %-18.2f | %-19.2f | %-14.2f | %-12.2f |\n",
  
        records->employee_num, records->employee_name, stats_code, records->hr,
        
        basic_salary, pay_overtime, records->deductions, net_pay);
}

// Function to print file records

void print_filerecord(FILE* fp) {

    // Display header
    
    printf("\n\t\t\t\t\t\t\t\t      ABC COMPANY\n"); 
    
    printf("\t\t\t\t\t\t\t\t      Makati City\n\n");
    
    printf("\n------------------------------------------------------------------------PAYROLL-------------------------------------------------------------------------\n");
    
    printf("|  Employee Number     |  Employee Name     |  Status    |  Hours Worked    |  Basic Salary      |  Overtime Pay       |  Deductions    |  Net Pay     |\n");
    
    printf("--------------------------------------------------------------------------------------------------------------------------------------------------------\n");

    char line[100]; // Buffer to store each line read from the file
    
    while (fgets(line, sizeof(line), fp) != NULL) {
    
        printf("%s", line); // to print the line read from the file
        
    }
    
    printf("--------------------------------------------------------------------------------------------------------------------------------------------------------\n");
    
}

// main 

int main() {

    FILE* fp = fopen("employee_records.txt", "a"); //append
    
    if (fp == NULL) { // to check if the file can be created
    
        printf("File Cannot Be Created!"); // to print if it is NULL
        
        return 1;
        
    }

// To declare variables and enter the number of employees

    int num;
    
    float basic_rate, basic_salary, pay_overtime, net_pay;
    
    printf("------------------------------------------------------\n");
    
    printf("		    EMPLOYEES DATA\n");
    
    printf("------------------------------------------------------\n\n");
    
    printf("Enter the Number of Employees: ");
    
    scanf("%d", &num);
    
    getchar(); // to clear newline character from the input buffer

    for (int i = 0; i < num; i++) {
    
        struct employee records;

        employee_details(&records);

        while (1) {
        
            if (records.stats_code == 'R') {
            
                printf("Enter Basic Salary Amount: ");
                
                scanf("%f", &basic_salary);

                if (records.hr > 160) {
                
                    basic_rate = basic_salary / 160;
                    
                    pay_overtime = compute_overtime(records.hr, basic_rate);
                    
                } else {
                
                    pay_overtime = 0;
                    
                }
                
            } else if (records.stats_code == 'C') {
            
                printf("Enter Basic Rate: ");
                
                scanf("%f", &basic_rate);

                if (records.hr <= 160) {
                
                    basic_salary = basic_rate * records.hr;
                    
                    pay_overtime = 0;
                    
                } else {
                
                    basic_salary = basic_rate * 160;
                    
                    pay_overtime = compute_overtime(records.hr, basic_rate);
                    
                }
                
            }
            

            break;
            
        }

// To calculate the net pay of the employees

        net_pay = compute_netpay(basic_salary, pay_overtime, records.deductions);
        
        print_employee_details(fp, &records, basic_salary, pay_overtime, net_pay);
        
    }

    fclose(fp); // to close a file
    
    fp = fopen("employee_records.txt", "r"); // to open a text file 
    
    if (fp == NULL) {
    
        printf("Error Opening File!");
        
        return 1;
        
    }

    print_filerecord(fp);
    

// To close a file

    fclose(fp);
    
    return 0;
    
}


