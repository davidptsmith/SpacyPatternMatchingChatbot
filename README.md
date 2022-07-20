# Spacy Pattern Matching Chatbot


### The Task:

The project is to create a rule based, job seeker chatbot to assist a user through conversation with as they look for a job. 

This project's dataset includes job descriptions written in natural language, as well as structured data on the city, job categories, and salary scale. The job descriptions will be processed as part of the project. 

A user can enquire about various jobs, filter based on input information and review those jobs that match their description. Due to the complexity, the chat bot possesses the ability to handle multi-turn discussion. 

### Data: 

The data provided is from the the job searching website Seek.com where many companies post looking for future employees. 

The csv for Australian job listings from Seek job board has 30,000 observations and 12 variables. 

- category	
- city	
- company_name	
- geo	
- job_board	
- job_description	
- job_title	
- job_type	
- post_date	
- salary_offered	
- state	
- url

Many of these have issues in the data such as character artifacts from text encoding and missing data. 

Job description field is the variable of interest, however, giving the various ways in which people write job descriptions there are many issues in this column. 
Luckily, where the job description lacks the information, the structured fields can work to combine the information and make it usable. 



# Data Processing

### TF-IDF Implemenetaiton 


Document Frequency (dft) is defined as the number of documents in the collection that contain a term.

Inverse Document Frequency (IDF) gives a bonus to words that appear frequently in a document but not across the full document collection. 
Essentially, a higher document frequency works against a word's rating.

The TF-IDF allows as to remove stop words and make similarity comparisons between documents. This can be used in ranking the users outputs or in classifying a piece of text across different categories. 

### Remove Stop Words & Place in Column

I am using the TF-IDF process to remove stop words and words that provide little meaning to text differentiation. The remaining words are then stored in a column within the dataframe. 

### Feature Engineering 

**Creating Salary Range**
Extract salary values and normalize the communication. For example, convert 70k into 70,000. 

**Creating Required Experience Column**
Extract information about the required experience for a role. This also involved converting titles such as junior, graduate, and senior into values for searching filters. 


# Rule Based Matching Functions



## Chat Bot Setup


## Bot Process: 
1. **Greet the user and await input**
2. **Parse input and determine input fields and values.**
    - if fields found: 
        - filter based on fields 
            - If many items found:
                - Ask user for more data and filter again 
                    - if yes:
                        - prompt user to add a filter 
                    - if no: 
                        - got to showing jobs
                    - if neither but filter detected: 
                        - go back and apply filter 
                    if cannot determine: 
                        - ask the user what they want to do
            - If a reasonable amount is returned: 
                - Ask the user to go through results 
                    - if yes:
                        - go to showing jobs 
                    - if no: 
                        - ask what they want to do
                    - if neither but another filter detected:
                        - go back and apply filter 
                    - if cannot determine:
                        - ask the user what they want to do
            - If no matches found:
                - Tell the user to broaden search 
    - else 
        - Ask the user to rephrase 
3. **Once user has confirmed they wish to see results, show top 5 and await input**
    - if input is 'next'
        - show the next five matches 
    - if the user enters a number
        - Show the job description to the user and await response
            - if back, return to jobs
0. **If at any time the user says "reset", "end" or other variant:**
    - clear values and start again. 
    
## Chatbot Outcome

Instead of typing into telegram again and again, this functions simulates the user inputs and can therefore run tests much more effectively. 

These functions could be extracted to formalized tests however, I did not have time to implement this. The logs show all the information required though. 



```diff

[33m---------------------------------[0m
[33m------- TEST SENTENCE 3 ---------[0m
-----------------------------------
USER INPUT:  [32m"I want a mining role in Port Hedland, find me a role there."[0m
-----------------------------------
Len after job_location:  217
Len after sort_job_title_by_similarity:  4
FOUND DATA: 
  Job Title:  mining 
  Expereince:  None 
  Location:  port hedland, karratha & pilbara 
  Contract:  None 
  Company:  None 
  Salary:  None
CURRENT INTENT:  ask_show_jobs

-----------------------------------
BOT OUTPUT: 
 [32m"You want a job Working in mining.
You want a job located in port hedland, karratha & pilbara. 

There are a few jobs for you, 4 in total. Would you like to see them?"[0m
-----------------------------------
-----------------------------------
USER INPUT:  [32m"yes"[0m
-----------------------------------
FOUND DATA: 
  Job Title:  mining 
  Expereince:  None 
  Location:  port hedland, karratha & pilbara 
  Contract:  None 
  Company:  None 
  Salary:  None
CURRENT INTENT:  ask_job_number_or_next

-----------------------------------
BOT OUTPUT: 
 [32m"No worries, lets go through what I found you. 

Here are jobs 1 - 3 out of the total 4 I could find.

    1) Underground Crusher Operator at Metals X Limited located in Port Hedland, Karratha & Pilbara on a Full Time contract

    2) Mining Superintendent at Fortescue Metals Group Ltd located in Port Hedland, Karratha & Pilbara on a Full Time contract

    3) Senior Mining Engineer at Mineral Resources Limited located in Port Hedland, Karratha & Pilbara on a Full Time contract

Type "next" to see the next set of jobs
Enter the number of the job that interests you and I'll show you the description and provide you with the link to apply."[0m
-----------------------------------
-----------------------------------
USER INPUT:  [32m"3"[0m
-----------------------------------
FOUND DATA: 
  Job Title:  mining 
  Expereince:  None 
  Location:  port hedland, karratha & pilbara 
  Contract:  None 
  Company:  None 
  Salary:  None
CURRENT INTENT:  ask_job_number_or_next

-----------------------------------
BOT OUTPUT: 
 [32m"The Company As an ASX 200 and perennial high performing company, Mineral Resources Limited (MRL) is an Australian based leader in the performance and delivery of diversified mining services and minerals processing, underpinned by a growing world-class portfolio of mining operations across multiple commodities including iron ore and lithium. Ground breaking innovation is at the heart of MRLs culture and is key to our enduring success. With a 1,600 strong growing workforce, a market capitalisation of $3Bn, revenue for FY17 of $1.46Bn, and a best in industry safety record, MRL will not only continue to provide exciting job prospects, it will also offer you the opportunity to start and finish your career with us. The Role & The Team In order to support our operational growth through 2018, we are seeking two suitably qualified Senior Mining Engineers to work back to back for our expanding Wodgina lithium operation in the Pilbara. Reporting to the Mining Manager, this role will be working a 9:5 or 5:2,4:3 roster from Perth, with an immediate start available. It is essential you hold a QM ticket for this role. This is a dynamic work environment which requires you to think fast, and roll your sleeves up. Your Responsibilities & Duties Accountable for all mine engineering, mine geology, survey and technical services duties on site Assist in the preparation of the site annual business plan and accountability for execution and meeting or exceeding the plan in all relevant areas Routine operational reports will be required including but not limited to; daily, weekly, monthly, mine reconciliation reports, reserve block model/grade Manage the day to day relationship with and provide guidance to Mine Operations team Maintain and foster a working environment which prompts safety in all that we do, whilst also providing outstanding service Your Experience & Skills Tertiary qualifications in Mining Engineering 7years + of mine operations experience in the resources industry WA unrestricted Quarry Manager Certificate of Competency is beneficial Competency with major recognized mining design and scheduling software such as Surpac, XACT, MinesShed, Trimble or similar High degree of communication, organisational and problem solving skills Demonstrate a detailed understanding of relevant costs and analyse to achieve optimal outcomes Strong analytical skills The Benefits 9:5 or 5:2,4:3 roster from Perth Backed by an experienced and supportive leadership team Work for an organisation with a first class reputation in camp and food, in the Pilbara and Goldfields region Be part of a high performing crew A workplace that lives and breathes a strong safety culture If you feel that you have the necessary attitude and skills for this role, apply now for a confidential chat with our friendly Recruitment Team. Shortlisting will commence immediately. Please note that no agency applications will be accepted at this time, unless specifically requested. Mineral Resources Limited is a proud Equal Opportunity Employer, and we encourage applicants who identify as Aboriginal or Torres Strait Islander to apply. This role is eligible for the MRL internal referral program.

Okay, for the Senior mining engineer role at Mineral resources limited and can be found at https://www.seek.com.au/job/35984913 the job description is above.

Type "back" if you want to see more jobs or "reset" to start over. 

Otherwise, good luck with your job hunt."[0m
-----------------------------------
-----------------------------------
USER INPUT:  [32m"back"[0m
-----------------------------------
FOUND DATA: 
  Job Title:  mining 
  Expereince:  None 
  Location:  port hedland, karratha & pilbara 
  Contract:  None 
  Company:  None 
  Salary:  None
CURRENT INTENT:  ask_job_number_or_next

-----------------------------------
BOT OUTPUT: 
 [32m"
Here are jobs 1 - 3 out of the total 4 I could find.

    1) Underground Crusher Operator at Metals X Limited located in Port Hedland, Karratha & Pilbara on a Full Time contract

    2) Mining Superintendent at Fortescue Metals Group Ltd located in Port Hedland, Karratha & Pilbara on a Full Time contract

    3) Senior Mining Engineer at Mineral Resources Limited located in Port Hedland, Karratha & Pilbara on a Full Time contract

Type "next" to see the next set of jobs
Enter the number of the job that interests you and I'll show you the description and provide you with the link to apply."[0m
-----------------------------------
-----------------------------------
USER INPUT:  [32m"next"[0m
-----------------------------------
FOUND DATA: 
  Job Title:  mining 
  Expereince:  None 
  Location:  port hedland, karratha & pilbara 
  Contract:  None 
  Company:  None 
  Salary:  None
CURRENT INTENT:  ask_job_number_or_next

-----------------------------------
BOT OUTPUT: 
 [32m"Those are all the jobs that I have to show you, would you like to select one of them too review or reset your search?"[0m
-----------------------------------
-----------------------------------
USER INPUT:  [32m"1"[0m
-----------------------------------
FOUND DATA: 
  Job Title:  mining 
  Expereince:  None 
  Location:  port hedland, karratha & pilbara 
  Contract:  None 
  Company:  None 
  Salary:  None
CURRENT INTENT:  ask_job_number_or_next

-----------------------------------
BOT OUTPUT: 
 [32m"Underground Crusher Operator Metals X Limited owns the Nifty Copper Operations and Wingellina Nickel Project, both in Western Australia. The Company is also Australias largest tin producer with its 50 per cent ownership of the Renison TinOperation in Tasmania. It continues to be a diversified group exploring and developing metals and minerals in Australia. Nifty Copper Mine is currently seeking expressions of interest from suitably qualified candidates for the position of Underground Crusher Operator. Competitive Salary Package 14 days on/7 days off roster, 12 hour shifts FIFO from Perth To be successful in the role you would need to have prior experience with Citech operated gyratory crusher. EXPERIENCE / QUALIFICATIONS: Conveyor and underground experience (essential) Bobcat ticket (desirable) IT ticket (desirable) Gantry crane ticket (desirable) Rigger ticket (desirable) Dogman ticket (desirable) Working at Heights ticket Successfully recognise and implement effective corrective actions in response to work hazards. Holding a current C Class Drivers Licence. You must be either an Australian citizen or a holder of an Australian permanent residency visa to be eligible to apply for this position.

Okay, for the Underground crusher operator role at Metals x limited and can be found at https://www.seek.com.au/job/35987688 the job description is above.

Type "back" if you want to see more jobs or "reset" to start over. 

Otherwise, good luck with your job hunt."[0m
-----------------------------------
-----------------------------------
USER INPUT:  [32m"end"[0m
-----------------------------------
BOT OUTPUT: 
 [32m"Is this the end? My only friend, the end? --- Goodbye ---"[0m

```