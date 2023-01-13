# New Companies 

### Problem: 

Amber's conglomerate corporation just acquired some new companies. Each of the companies follows this hierarchy:

![image](https://user-images.githubusercontent.com/48019306/211897614-fa694f0c-6820-4a63-945c-f55e1904c77f.png)

Given the table schemas below, write a query to print the company_code, founder name, total number of lead managers, total number of senior managers, total number of managers, and total number of employees. 
Order your output by ascending company_code.
  
**Note:**
- The tables may contain duplicate records.
- The company_code is string, so the sorting should not be numeric. 
For example, if the company_codes are C_1, C_2, and C_10, then the ascending company_codes will be C_1, C_10, and C_2.

The following tables contain company data:

**Company**: The company_code is the code of the company and founder is the founder of the company.

![image](https://user-images.githubusercontent.com/48019306/211898393-c485e515-61fe-48ee-a7a5-f36d0bd7d8ed.png)

**Lead_Manager**: The lead_manager_code is the code of the lead manager, and the company_code is the code of the working company.

![image](https://user-images.githubusercontent.com/48019306/211898501-8b66173b-fdab-4972-b78e-991de5fe25b5.png)

**Senior_Manager**: The senior_manager_code is the code of the senior manager, the lead_manager_code is the code of its lead manager, 
and the company_code is the code of the working company. 

![image](https://user-images.githubusercontent.com/48019306/211898710-81556cc1-46f8-4e2e-8daa-acfcf17409e9.png)

**Manager**: The manager_code is the code of the manager, the senior_manager_code is the code of its senior manager, the lead_manager_code is the code of its lead manager, and the company_code is the code of the working company.

![image](https://user-images.githubusercontent.com/48019306/211898894-15797094-aa9c-4223-9180-02d4d9a46600.png)

**Employee**: The employee_code is the code of the employee, the manager_code is the code of its manager, the senior_manager_code is the code of its senior manager, the lead_manager_code is the code of its lead manager, and the company_code is the code of the working company.

![image](https://user-images.githubusercontent.com/48019306/211899051-750b7b8c-9067-4b69-b4a0-82d5e54fc2c7.png)

### My answer: 

````sql   
SELECT company_code, 
       founder,
      (SELECT COUNT(DISTINCT lead_manager_code) 
         FROM lead_manager l 
        WHERE l.company_code = c.company_code) lead_manager,
      (SELECT COUNT(DISTINCT senior_manager_code) 
         FROM senior_manager l 
        WHERE l.company_code = c.company_code) senior_manager,
      (SELECT COUNT(DISTINCT manager_code) 
         FROM manager l 
        WHERE l.company_code = c.company_code) manager,
      (SELECT COUNT(DISTINCT employee_code) 
         FROM employee l 
        WHERE l.company_code = c.company_code) employee
  FROM company c
 ORDER BY company_code;
```` 
