---
layout: post

title: Mental Health Issue Tracker for Tech Company.
---
Mental Health issue is a global challenge affecting people of every age, background and profession. According to Statistics Canada, mental illnesses cost Canadian employers billions of dollars in absenteeism or sick days, “presenteeism” (coming to work, even when the employee can’t work well), disability and other benefits, and lost productivity. Many times, employees who are already suffering from the issue do not reach out for help because either they are not comfortable with the benefit plan or they are not aware about it, which eventually affects their performance. If employers could get the information of how many employees are dealing with the issue, are actually reaching out for help, how much they are aware about the policies, and what is the intensity of work interference their health has, then it might be possible for the employer to enhance/add policies to spread more awareness and help employees. To address this situation, we propose to build an application for the employer which will provide them the information of how many of the employees are aware about the benefits plans. Also, it will filter on variables to show ratio about mentally ill employees who need help with respect to their health history. Also, the application will enable opportunities for the employer to understand the intensity of effect the illness is causing to their performance with respect to treatment received or not.

#### Data :   

We visualized a dataset Mental Health Survey in Tech Company 2016(Kaggle dataset), which has 1434 responses. Every response has 39 variables that describe the benefits the employers provide, how much employees(‘ID’, ‘Country’, ‘Age’, Gender’) are aware of those benefits and include them, employees mental health history(‘Family_hist’, ‘Have you had a mental health disorder in the past?’), their current health situation (‘Do you currently have a mental health disorder?’), if they are diagnosed by professional and seek help from the professional (‘Have you been diagnosed with a mental health condition by a medical professional?’,’ Have you ever sought treatment for a mental health issue from a mental health professional?’), also how much it affects their work when treated and when not treated effectively (‘If you have a mental health issue, do you feel that it interferes with your work when being treated effectively?’,’ If you have a mental health issue, do you feel that it interferes with your work when NOT being treated effectively?‘). The data also contains other information, such as whether employees feel comfortable discussing the issue with their supervisor, co-workers and how they think it might affect their professional career if discussed.

#### Shinny app design :    

* Tab rationale

__‘Usage’__     
Our landing page which is ‘Usage’ tab gives a brief description about the various other tabs of the application, that is our motivation , overview , analysis and data tab.   

![img](/images/Shn/img1.PNG)

__‘Overview’__  
Overview tab gives the most common mental health issues among the employees in the form of a word cloud. We implemented wordcloud2 which shows the most frequently occurring mental health disorders. Along with that, for the employer to get a clear idea about awareness among the employees regarding the benefit options given by the organization for mental health we generated a plot with the count of employees with respect to their awareness. We used bar plots to provide the visual. 

![img](/images/Shn/img2.PNG)

__‘Analysis’__  
Analyzing the data to know more about the employee’s past and present health diagnoses is as important as to know if they have been treated clinically. This will give the employer the information of how many employees need more encouragement to seek professional help. To visualize these we developed two charts, one which depicts employee with medical illness history with respect to if they have been clinically diagnosed. Another chart depicts medical history with respect to if they are getting professional help.
Mental health has adverse effect in work and performance of employees. To understand that we developed two charts , one which depicts how much mental health disorder interferes with work when treated by professional, another how much it effect when not treated by professional. Both the charts are bar charts giving the count of responses of employees depicting the interference of mental health issue in their work.

![img](/images/Shn/img3.PNG)

__'Data'__  
To see the raw data which contains the survey responses for detail analysis.   

**Variable Description**  

| Name of variable| Description |
|-----------------|-------------|
|Condition|Mental health disorder names |
|Age |Age of employees|
|work_country |Country of their work |
|options_for_seeking_help|Are you aware of the options of seeking help for mental health issue?|
|scared_of_discussing_with_employer |Are you scared of discussing your mental health issue with your employer?|
|Treatment_sought |Have you sought treatment from professional? |
|Clinically_diagnosed |Have you been diagnosed with mental health issue by a professional?|
|wrk_interference_when_treated|How much your mental health issue effects your work when treated by professional?|
|wrk_interference_No_treatement|How much your mental health issue effects your work when not  treated by professional?|
|MHD|Presence of mental health disorder in past and present /past or present /only past /only present / not applicable.|

![img](/images/Shn/img4.PNG)

* 'Filter rationale'    

All tabs except for ‘Usage’ , has provision of filtering the data . The three filters which could be used are ‘Age’, ‘Gender’ , ‘Country’.


#### Notes : 

* The application [app](https://marcelle-sayanti.shinyapps.io/mental_health_issue_tracker/)   
* The R code : [Script](https://github.com/UBC-MDS/Mental_Health_Issue_Tracker/blob/master/src_shiny/app.R)    
* The GitHub repo [Repo](https://github.com/UBC-MDS/Mental_Health_Issue_Tracker)     

* This app is designed by Sayanti Ghosh , Marcelle Chiriboga as part of MDS course project.   


