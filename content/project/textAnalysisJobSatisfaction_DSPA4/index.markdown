---
title: "Is job satisfaction in the description?"	
subtitle: "Analyzing job description text data to infer job satisfaction and stress"
summary: "Analyzing job description text data to infer job satisfaction and stress"
date: 2018-10-01
author: "<h3>Susannah</h3>"	
output:
  html_document:
    keep_md: true
    theme: cerulean
    highlight: monochrome
# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder. 
image:
  # Caption (optional)
  caption: ""
  # Focal point (optional)
  # Options: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight
  focal_point: "Center"
  # Show image only in page previews?
  preview_only: false
tags:
- Analysis
- Visualization
---
# Background
This is a homework assignment I completed for a graduate course in [Data Science and Predictive Analysis](http://www.socr.umich.edu/people/dinov/2018/Fall/DSPA_HS650/) at the University of Michigan in Fall 2018.

**Problem 4.1**

Construct an R protocol to examine the job-stress level (**Stress_Level**) and hiring-potential (**Hiring_Potential**) using the job description (JD) (**Description**) text.

# Set-Up Workspace 

## Dataset Description 
- The data we are using for homework 4 is the [SOCR Dataset Ranking the Best and Worst USA Jobs for 2011](http://wiki.socr.umich.edu/index.php/SOCR_Data_2011_US_JobsRanking#2011_Ranking_of_the_200_most_common_Jobs_in_the_US). 
- The variables in this data are
    - **Index**: Ranks of the 200 most common jobs based on the Overall_Score variable (lower rank/index indicates better job, and higher rank indicates a less desirable job for 2011). Overall Rank Index represents the sum of the rankings in each of the five variable (below) where each of the variables are assumed (but this may not necessarily be true) to be equally important.
    - **Job Title**: title of the profession 
    - **Overall_Score**: A *Dependent variable* corresponding to the linearly predicted overall rank of this job using the remaining explanatory variables (below).
    - **Average_Income** (USD): An estimate of the mid-levels income for this profession.
    - **Work_Environment**: Work environment for each job reflects 2 basic factors: physical and emotional components. Points are assigned for potential for adverse working conditions (larger scores). In other words, fewer work-environment points would yield a lower (better) job rank and higher points reflect lower quality environments.
    - **Stress_Level**: Expected level of job-related stress dependent on 11 potential stress factors. A high stress score implies that stress is a major part of the job, and lower stress score indicates lower job-relates stress. 
    - **Stress_Category**: Categorical data where lower number seems to correspond to less stress while higher number is higher stress
    - **Physical_Demand**: The total physical demand if a job. Higher number means more physically demanding.
    - **Hiring_Potential**: The hiring-potential rank assigns higher scores to jobs with promising future outlook and lower scores for less future potential. 
    - **Description**: A brief job description for each profession.
    
## Download the data

```r
# install.packages('rvest')
library(rvest)  #web scraping package

wiki_url <- read_html('http://wiki.socr.umich.edu/index.php/SOCR_Data_2011_US_JobsRanking#2011_Ranking_of_the_200_most_common_Jobs_in_the_US')

job_data <- html_table(html_nodes(wiki_url, "table")[[1]])
```

## View the data

```r
# review data
library(DT)
```

```
## Warning: package 'DT' was built under R version 3.4.4
```

```r
datatable(job_data, options = list(
  autoWidth = TRUE,
  columnDefs = list(list(width = '100%', targets = c(1, 3)))
))
```

<!--html_preserve--><div id="htmlwidget-322504bc3aa7d6ab8cc3" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-322504bc3aa7d6ab8cc3">{"x":{"filter":"none","data":[["1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16","17","18","19","20","21","22","23","24","25","26","27","28","29","30","31","32","33","34","35","36","37","38","39","40","41","42","43","44","45","46","47","48","49","50","51","52","53","54","55","56","57","58","59","60","61","62","63","64","65","66","67","68","69","70","71","72","73","74","75","76","77","78","79","80","81","82","83","84","85","86","87","88","89","90","91","92","93","94","95","96","97","98","99","100","101","102","103","104","105","106","107","108","109","110","111","112","113","114","115","116","117","118","119","120","121","122","123","124","125","126","127","128","129","130","131","132","133","134","135","136","137","138","139","140","141","142","143","144","145","146","147","148","149","150","151","152","153","154","155","156","157","158","159","160","161","162","163","164","165","166","167","168","169","170","171","172","173","174","175","176","177","178","179","180","181","182","183","184","185","186","187","188","189","190","191","192","193","194","195","196","197","198","199","200"],[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,27,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,53,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,101,102,103,104,105,106,107,108,109,110,111,112,113,114,115,116,117,118,119,120,121,122,123,124,125,126,127,128,129,130,131,132,133,134,135,136,137,138,139,140,141,142,143,144,145,146,147,148,149,150,151,152,153,154,155,156,157,158,159,160,161,162,163,164,165,166,167,168,169,170,171,172,173,174,175,176,177,178,179,180,181,182,183,184,185,186,187,188,189,190,191,192,193,194,195,196,197,198,199,200],["Software_Engineer","Mathematician","Actuary","Statistician","Computer_Systems_Analyst","Meteorologist","Biologist","Historian","Audiologist","Dental_Hygenist","Sociologist","Accountant","Paralegal_Assistant","Physicist","Financial_Planner","Philosopher","Occupational_Therapist","Parole_Officer","Aerospace_Engineer","Economist","Speech_Pathologist","Astronomer","Loan_Officer","Petroleum_Engineer","Dietitian","Technical_Writer","Optometrist","Computer_Programmer","Librarian","Medical_Technologist","Stenographer/Court_Reporter","Chiropractor","Civil_Engineer","Medical_Secretary","Medical_Laboratory_Technician","Pharmacist","Industrial_Engineer","Purchasing_Agent","Bookkeeper","Medical_Records_Technician","School_Principal","Dental_Laboratory_Technician","Tax_Examiner/Collector","Web_Developer","Physical_Therapist","Vocational_Counselor","Motion_Picture_Editor","Physiologist","Nuclear_Engineer","Insurance_Underwriter","Industrial_Designer","Orthodontist","Museum_Curator","Judge","Personnel_Recruiter","Geologist","Podiatrist","Market_Research_Analyst","Jeweler","Anthropologist","Architectural_Drafter","Mechanical_Engineer","Optician","Archeologist","Veterinarian","Typist/Word_Processor","Cosmetologist","Clergy","Chemist","Musical_Instrument_Repairer","Zoologist","Conservationist","Social_Worker","Set_Designer","Dentist","Barber","Occupational_Safety/Health_Inspector","Electrical_Engineer","Psychologist","Publication_Editor","Author","Attorney","Physician_(General_Practice)","Engineering_Technician","Physician_Assistant","Bank_Teller","Artist_(Fine_Art)","Respiratory_Therapist","Bookbinder","Photographic_Process_Worker","Teacher's_Aide","Psychiatrist","Heating/Refrigeration_Mechanic","Nurse_(Registered)","Sewage_Plant_Operator","Hotel_Manager","Telephone_Installer/Repairer","Industrial_Machine_Repairer20","Broadcast_Technician","Teacher","Surgeon","Electrical_Equipment_Repairer","Insurance_Agent","Receptionist","Cashier","Advertising_Account_Executive","Vending_Machine_Repairer","Architect","Electrical_Technician","Computer_Service_Technician","Stockbroker","Forklift_Operator","Public_Relations_Executive","Corporate_Executive_(Senior)","Construction_Foreman","Piano_Tuner","Furniture_Upholsterer","Communications_Equipment_Mechanic","Agricultural_Scientist","Compositor/Typesetter","Commercial_Airline_Pilot","Appliance_Repairer","Office_Machine_Repairer","Undertaker","Real_Estate_Agent","Telephone_Operator","Nurse_(Licensed_Practical)","Newscaster","Surveyor","Shipping/Receiving_Clerk","Dressmaker","Tool-And-Die_Maker","Guard","Automobile_Assembler","Precision_Assembler","Railroad_Conductor","Machine_Tool_Operator","Machinist","Waiter/Waitress","Bartender","Janitor","Plumber","Fashion_Designer","Photographer","Shoe_Maker/Repairer","Nurse's_Aide","Recreation_Worker","Electrician","Flight_Attendant","Glazier","Nuclear_Decontamination_Technician","Automobile_Body_Repairer","Sales_Representative_(Wholesale)","Drill-Press_Operator","Aircraft_Mechanic","Buyer","Chauffeur","Farmer","Salesperson_(Retail)","Corrections_Officer","Drywall_Applicator/Finisher","Carpet/Tile_Installer","Actor","Stationary_Engineer","Bus_Driver","Highway_Patrol_Officer","Dishwasher","Bricklayer","Maid","Advertising_Salesperson","Truck_Driver","Travel_Agent","Construction_Machinery_Operator","Mail_Carrier","Carpenter","Firefighter","Disc_Jockey","Police_Officer","Garbage_Collector","Choreographer","Plasterer","Butcher","Automobile_Mechanic","Dairy_Farmer","Photojournalist","Child_Care_Worker","Sheet_Metal_Worker","Reporter_(Newspaper)","Sailor","Stevedore","Construction_Worker","Meter_Reader","Painter","Welder","Emergency_Medical_Technician","Taxi_Driver","Roofer","Lumberjack","Ironworker","Roustabout"],[60,73,123,129,147,175,182,192,195,197,200,216,217,217,223,225,230,236,240,247,251,252,256,257,270,270,273,273,282,285,292,296,297,298,299,302,307,309,311,312,312,317,330,332,333,338,339,349,352,355,360,365,367,367,370,372,373,375,376,380,390,392,395,398,399,402,404,410,411,413,422,423,424,428,431,432,433,437,438,446,448,448,449,462,463,469,477,482,483,491,495,496,504,504,506,506,506,507,518,528,528,529,531,531,533,536,538,542,542,547,548,548,552,556,558,559,560,562,564,565,566,571,573,574,578,581,583,583,585,585,590,595,600,600,604,605,616,621,621,621,626,628,633,638,638,642,643,643,643,645,645,649,650,651,655,661,665,666,666,673,678,679,680,681,686,692,695,699,700,701,711,716,720,725,729,730,730,731,731,738,742,743,749,751,753,769,770,777,780,786,798,798,798,811,814,821,863,868,887,892],[87140,94178,87204,73208,77153,85210,74278,63208,63144,67107,70122,60174,47153,106196,101164,61221,70193,47155,95130,87240,65143,105233,55239,109147,52127,63170,96163,71176,54197,55100,48256,68358,76139,30110,36129,109070,75122,55165,33138,31148,85119,35176,49214,83086,74104,53171,51352,51181,97109,58175,58200,121065,48211,113439,46211,81274,116249,62229,34200,53175,46141,77136,33143,51190,81198,33104,23169,43226,68195,33211,57166,54114,40154,45219,142144,24169,63145,83135,66179,51246,54279,113211,192065,55142,84105,24083,44335,53085,31194,26182,23119,160242,41154,64114,40148,46207,58134,44131,33272,51132,365258,51132,46342,25118,18067,62105,30147,73193,32969,37164,67470,30114,90160,161141,82210,33229,30147,56132,34150,33165,106153,34152,38157,54203,40357,31163,40090,50456,54197,28132,27147,47122,24135,27263,28150,54145,34122,38138,18100,18107,22125,46182,61291,30265,23112,24089,22138,47176,40184,36186,37163,38178,51308,31124,53118,49207,22119,59222,20153,39150,37179,37238,49116,51139,34167,53171,18053,47175,19093,43309,38128,31147,40169,52042,39184,45222,28375,49185,32174,38283,38160,29150,35200,32114,40209,19100,41208,34275,36150,39186,29211,34171,34152,35126,30168,21127,34168,32109,34127,32143],[150,89.72,179.44,89.52,90.78,179.64,314.37,136.41,463.43,593.25,322.24,276.78,263.82,269.46,225,361.44,505.56,381.06,230.3,227.35,463.43,314.37,390.56,322.42,334.4,221.25,538.34,265.08,385.2,371.7,361.44,587.28,322.42,318.29,454.3,788.64,230.3,230.85,204.4,165.2,432.99,404.1,449.1,275,716.21,556.4,274.68,359.28,322.42,228.1,177,940.59,428,655.98,737.85,449.1,783.04,863.93,316.4,500.17,222.95,278.46,371.7,545.64,978.8,160.64,581.14,626.52,449.1,234.7,356.72,673.65,550.42,354,1119.75,575.12,538.92,318.92,727.52,486.75,752.25,1261.5,1471.5,493.79,1295.8,642.56,486.75,884.73,359.2,404.1,546.78,1417,633.5,1018.75,734.24,807.12,657.16,422.46,351.76,709.6,1962,657.16,1085.52,665.21,595.36,1010.1,469.4,1195,502.26,610.22,1155.75,454.5,1247.13,1540,923.21,563.28,504.36,610.22,494.01,440.9,1064.16,610.22,611.91,1015.4,1293.57,719.82,1080,929.25,956.97,520.32,704.31,740.55,812.06,639.45,639.45,830.28,753.1,756.8,687.42,796.74,489.6,836.38,929.25,663.75,597.24,872.96,889.14,855.76,803,796.86,821.88,669.9,1293.84,728.32,981.2,1108.08,945.8,1263.84,836.43,1646.75,973.94,796.86,1371.75,1055.47,1124.48,1733.4,868.32,1151.02,682.02,1293.57,968.2,1134.77,1136.25,1461.24,973.36,3314.03,1062,1877.85,1368.32,1106.25,929.67,987.8,1100.55,1474.48,1327.5,947.25,1266.14,1106.25,1660.56,1084.59,1555.85,1127.36,1034.02,1180.14,1610.7,2317.21,1481.2,1817.53,1593.72,1731.45],[10.4,12.78,16.04,14.08,16.53,15.1,15.78,17.08,9.44,12.07,19.47,19.74,16.53,16.96,18.64,12.56,13.22,12.55,20.3,17.4,12.43,21.33,14.11,18.93,10.27,18.7,16.63,11.76,11.48,16,18.56,13.58,22.39,15.1,9.29,15.7,20.22,19.65,12.38,7.48,25.19,11.76,17.14,21.86,14.04,14.71,20.52,16.81,32.09,16.75,25,22.65,21.11,21.39,20.11,23.74,18.49,32.29,8,26.75,17.41,22.36,15.43,26.9,15.98,13.04,10.69,21.26,16.95,8.11,19.66,14.14,24.54,19.19,23.44,8.69,17.45,23.35,28.79,25.46,29.79,36.11,25.65,23.42,32.05,12.83,23.35,19.85,5.94,6.82,14.19,24.42,20.54,30.14,17.48,23.07,14.34,16.31,17.72,27.32,30.58,16.32,36.42,23.18,12.67,41.05,11.47,39.93,8.69,12.64,39.7,10.14,47.6,47.41,31.1,14.29,10.47,24.32,19.5,16.65,59.53,12.52,15.57,29.03,38.57,15.63,25.9,43.56,27.97,14.32,7.47,14.22,16.35,8.63,9.5,32.45,14.22,13.38,15,13.07,15.25,22.82,36.91,28.65,17.12,22.89,20.38,20.76,44.84,26.86,34.63,18.78,37.08,11.24,27.18,37.07,21.19,26.22,21.53,31.5,19.79,18.38,45.16,27.39,25.67,40.71,14.53,20.75,18.93,36.09,28.28,40.47,23.69,23.42,28.84,60.22,29.75,43.85,23.74,38.83,24.6,19.5,26,26.14,47.09,26,29.08,44.75,29.5,24.86,30.11,21.71,31.52,23.26,39.68,46.27,30.68,40.09,31.27,26.43],[1,1,1,1,1,1,1,1,0,1,1,1,1,1,1,1,1,1,2,1,1,2,1,1,1,1,1,1,1,1,1,1,2,1,0,1,2,1,1,0,2,1,1,2,1,1,2,1,3,1,2,2,2,2,2,2,1,3,0,2,1,2,1,2,1,1,1,2,1,0,1,1,2,1,2,0,1,2,2,2,2,3,2,2,3,1,2,1,0,0,1,2,2,3,1,2,1,1,1,2,3,1,3,2,1,4,1,3,0,1,3,1,4,4,3,1,1,2,1,1,5,1,1,2,3,1,2,4,2,1,0,1,1,0,0,3,1,1,1,1,1,2,3,2,1,2,2,2,4,2,3,1,3,1,2,3,2,2,2,3,1,1,4,2,2,4,1,2,1,3,2,4,2,2,2,5,2,4,2,3,2,1,2,2,4,2,2,4,2,2,3,2,3,2,3,4,3,4,3,2],[5,3.97,3.97,3.95,5.08,6.98,4.98,5.09,7.43,7,5.09,4.23,5.79,7.98,4,6.04,8.43,6.47,6.21,4.09,9.43,7.98,4.76,8.21,6.36,5.85,9.79,5.84,7.56,7.26,7.03,12.79,10.21,6.06,7.26,8.86,8.21,7.23,5.18,7.26,5.62,7.98,7.98,10,15.43,8.56,7.16,7.98,6.21,7.12,6.85,8.96,9.56,5.09,5.84,12.98,10.79,4.09,6.04,9.09,6.92,10.28,8.26,9.09,17.79,5.03,9.3,7.44,9.98,9.39,14.19,15.98,5.47,8.85,9.96,9.22,14.98,11.11,7.09,4.85,6.85,6.09,10.9,7.98,9.36,6.03,9.85,13.43,8.98,7.98,7.41,11.9,20.05,13.15,18.18,6.97,15.39,16.39,8.79,11.87,15.9,13.39,6.05,6,7,4.62,14.39,6.92,12.13,12.39,7.25,14.09,7.24,6,20.72,13.39,14.41,14.39,13.98,10.82,10.87,14.39,12.41,15.15,8.58,9,15,6.85,24.11,9.67,13.29,16.87,12.55,13.53,13.53,14.77,14.86,17.46,11,13,16.16,21.8,9.85,10.85,9.53,17,18.47,22.01,10.03,18.85,20.13,17.57,9.58,17.1,18.92,8.23,13.46,34.53,11,13.41,27.85,17.85,8.85,26.18,12.03,17.63,22,32.85,17,9.58,21.68,6,28.09,26.86,25.46,43.23,7.85,22.63,36.55,11.85,25.85,19.98,21.57,30.53,15.85,19,30.73,9.85,30.77,28.03,36.41,25.67,26,29.08,21.26,14.46,33.46,38.87,36.85,36.89],[27.4,19.78,17.04,11.08,15.53,12.1,11.78,11.08,21.44,33.07,19.63,16.74,23.53,11.96,3.64,8.56,24.22,17.55,8.3,-0.6,17.43,14.33,5.39,10.47,4.27,4.7,19.93,-7.24,6.48,10,17.56,21.58,22.39,24.1,14.29,14.7,6.22,6.65,4.38,6.48,7.19,12.76,12.14,8.86,28.04,9.71,6.52,5.81,5.09,-5.25,1,17.65,19.11,-1.61,15.11,14.74,9.49,21.29,-8,25.75,-3.59,1.36,8.43,25.9,32.98,-16.96,15.69,14.26,-2.05,-2.89,5.66,10.14,7.54,1.19,14.44,7.69,6.45,-2.65,6.79,-3.54,8.79,10.11,20.65,-2.58,37.05,-0.17,6.35,12.85,-24.06,-20.18,-0.81,9.42,17.54,20.14,15.48,1.07,-3.66,-0.69,-9.28,8.32,20.58,-3.68,9.42,4.18,-9.33,-0.95,-3.53,5.93,-14.31,-11.36,4.7,-7.86,2.6,-2.59,10.1,-3.71,-6.53,-2.68,0.5,-12.35,5.53,-10.48,-11.43,11.03,16.57,-8.37,18.9,-0.44,13.97,-17.68,-10.53,-12.78,3.35,-14.37,-14.5,5.45,-6.78,-12.62,-5,-2.93,-6.75,3.82,-7.09,2.65,-31.88,10.89,10.38,-1.24,2.84,6.86,13.63,-11.22,1.08,-40.76,1.18,-3.93,5.19,5.22,0.53,9.5,0.79,-13.62,-0.84,3.39,0.67,7.71,-2.47,-1.25,-5.07,-2.91,2.28,-8.53,0.69,-3.58,-0.16,18.22,-11.25,7.85,8.74,-4.17,-6.4,-5.5,-2,8.14,2.09,0,-0.92,-14.25,4.5,-14.14,7.11,-30.29,-3.48,-15.74,4.68,5.27,-9.32,0.09,-12.73,-19.57],["Researches_designs_develops_and_maintains_software_systems_along_with_hardware_development_for_medical_scientific_and_industrial_purposes","Applies_mathematical_theories_and_formulas_to_teach_or_solve_problems_in_a_business_educational_or_industrial_climate","Interprets_statistics_to_determine_probabilities_of_accidents_sickness_and_death_and_loss_of_property_from_theft_and_natural_disasters","Tabulates_analyzes_and_interprets_the_numeric_results_of_experiments_and_surveys","Plans_and_develops_computer_systems_for_businesses_and_scientific_institutions","Studies_the_physical_characteristics_motions_and_processes_of_the_earth's_atmosphere","Studies_the_relationship_of_plants_and_animals_to_their_environment","Analyzes_and_records_historical_information_from_a_specific_era_or_according_to_a_particular_area_of_expertise","Diagnoses_and_treats_hearing_problems_by_attempting_to_discover_the_range_nature_and_degree_of_hearing_function","Assists_dentists_in_diagnostic_and_therapeutic_aspects_of_a_group_or_private_dental_practice","Studies_human_behavior_by_examining_the_interaction_of_social_groups_and_institutions","Prepares_and_analyzes_financial_reports_to_assist_managers_in_business_industry_and_government","Assists_attorneys_in_preparation_of_legal_documents;_collection_of_depositions_and_affidavits;_and_investigation_research_and_analysis_of_legal_issues","Researches_and_develops_theories_concerning_the_physical_forces_of_nature","Related_to_careers_in_portfolio_management_the_financial_planner_offers_a_broad_range_of_services_aimed_at_assisting_individuals_in_managing_and_planning_their_financial_future","Studies_questions_concerning_the_nature_of_intellectual_concepts_and_attempts_to_construct_rational_theories_concerning_our_understanding_of_the_world_around_us","Develops_individualized_programs_of_activity_for_mentally_physically_developmentally_and_emotionally_impaired_persons_to_aid_them_in_in_achieving_self-reliance","Monitors_counsels_and_reports_on_the_progress_of_individuals_who_have_been_released_from_correctional_institutions_to_serve_parole","Designs_develops_and_tests_new_technologies_concerned_with_the_manufacture_of_commercial_and_military_aircraft_and_spacecraft","Studies_and_analyzes_the_effects_of_resources_such_as_land_labor_and_raw_materials_on_costs_and_their_relation_to_industry_and_government","Assesses_hearing_speech_and_language_disabilities_and_provides_treatment._Assists_individuals_with_communication_disorders_through_diagnostic_techniques","Uses_principles_of_physics_and_mathematics_to_understand_the_workings_of_the_universe","Loan_officers_help_people_apply_for_loans._This_lets_people_do_things_like_buy_a_house_or_a_car_or_pay_for_college","Plans_drilling_locations_and_effective_production_methods_for_optimal_access_to_oil_and_natural_gas","Assesses_patients'_dietary_needs_plans_menus_and_instructs_patients_and_their_families_about_proper_nutritional_care","Transforms_scientific_and_technical_information_into_readily_understandable_language","Diagnoses_visual_disorders_and_prescribes_and_administers_corrective_and_rehabilitative_treatments","Organizes_and_lists_the_instructions_for_computers_to_process_data_and_solve_problems_in_logical_order","Selects_and_organizes_materials_to_make_information_available_to_the_public","Performs_lab_analysis_for_diagnosis_and_treatment_of_disease","Transcribes_testimony_judicial_decisions_and_other_proceedings_of_a_court_of_law","Treats_physical_problems_by_manipulating_various_parts_of_the_body_especially_the_spinal_column","Plans_and_supervises_the_building_of_roads_bridges_tunnels_and_buildings","Transcribes_dictations_prepares_correspondence_and_assists_physicians_and_other_medical_scientists_in_compiling_reports_articles_speeches_and_conference_proceedings","Conducts_routine_laboratory_tests_and_analysis_used_in_the_detection_diagnosis_and_treatment_of_disease","Advises_physicians_and_patients_on_the_affects_of_drugs_and_medications;_prepares_and_dispenses_prescriptions","Plans_for_optimum_use_of_facilities_and_personnel_to_improve_industrial_efficiency","Assesses_an_establishment's_needs_and_available_products_and_buys_raw_materials_equipment_and_machinery_furniture_and_other_supplies_accordingly","Maintains_financial_records_and_prepares_statements_of_a_company's_income_and_daily_operating_expenses","Maintains_complete_accurate_and_up-to-date_medical_records_for_use_in_treatment_billing_and_statistical_surveys","Supervises_the_educational_curricula_and_day-to-day_activities_in_elementary_and_secondary_schools_as_well_as_colleges_and_universities","Makes_and_repairs_dentures_crowns_and_orthodontic_devices","Determines_tax_liability_and_collects_taxes_from_individuals_or_businesses","Creating_and_maintaining_layout_navigation_and_interactivity_of_intranet_and_internet_websites","Plans_and_directs_treatment_to_improve_mobility_and_alleviate_pain_in_persons_disabled_by_injury_or_disease","Helps_students_with_educational_and_social_problems_and_provides_vocational_guidance","Supervises_the_filming_and_editing_of_motion_pictures_for_entertainment_business_and_educational_purposes","Studies_the_life_functions_of_plants_and_animals_both_in_their_natural_habitats_and_under_experimental_or_abnormal_conditions","Conducts_research_designs_and_monitors_the_operation_and_maintenance_of_nuclear_reactors_and_power_plant_equipment","Assesses_and_analyzes_the_risks_inherent_in_insuring_potential_policy_holders_before_making_recommendations_to_the_insurance_companies_that_employ_them","Designs_and_develops_manufactured_products","Diagnoses_and_corrects_deviations_in_dental_growth_development_and_position","Plans_and_directs_activities_of_workers_and_arranges_for_the_exhibition_of_articles_of_interest_to_museum_visitors","Arbitrates_legal_matters_coming_under_the_jurisdiction_of_the_federal_government_using_a_thorough_knowledge_of_federal_statutes_and_legal_precedent","Interviews_prospective_employees_administers_tests_and_provides_information_about_company_policies_and_available_jobs","Studies_and_analyzes_the_physical_properties_of_the_earth's_surface_adding_to_our_knowledge_of_oil_and_gas_exploration_techniques","Diagnoses_and_treats_problems_of_the_feet_through_corrective_devices_medication_therapy_and_surgery","Collects_and_evaluates_data_to_make_recommendations_to_businesses_concerning_trends_in_consumer_purchasing","Manufactures_and_repairs_rings_bracelets_pins_and_necklaces_using_precious_or_semi-precious_metals_and_stones","Studies_the_social_customs_language_and_physical_attributes_of_people_throughout_the_world","Prepares_drawings_according_to_the_specifications_of_scientists_architects_and_engineers","Develops_mechanical_products_and_coordinates_the_operation_and_repair_of_power-using_and_power-producing_machinery","Fills_lens_prescriptions_and_fits_eyeglasses_and_contact_lenses","Studies_analyzes_and_collects_data_based_on_research_of_ancient_often_preliterate_cultures","Ministers_to_the_care_of_animals_through_the_use_of_preventative_and_diagnostic_techniques","Prepares_and_edits_typed_or_electronic_copies_of_handwritten_printed_or_magnetically_recorded_documents","Creates_hair_styles_and_advises_clients_about_caring_for_their_hair_between_appointments","Leads_a_congregation_in_worship_and_other_spiritual_services_provides_moral_guidance_to_members_and_participates_in_community_outreach","Develops_substances_through_research_of_properties_and_composition;_assists_industry_and_individuals_by_study_of_chemical_structures_of_products","Maintains_and_repairs_band_and_orchestral_instruments_of_all_kinds","Studies_the_behavior_life_processes_origins_and_diseases_of_animals","Conducts_research_into_range_problems_and_manages_range_lands_to_make_efficient_use_of_livestock_and_wildlife_without_destroying_their_habitat","Assists_individuals_families_and_groups_in_need_of_counseling_and_special_social_services","Plans_and_assembles_theatrical_sets_for_film_television_and_stage_productions","Examines_cleans_and_repairs_teeth_and_diagnoses_and_treats_diseases_and_abnormalities_of_the_mouth","Shampoos_trims_cuts_and_styles_hair_according_to_the_desires_of_customers","Examines_places_of_business_to_ensure_that_equipment_and_work_conditions_do_not_endanger_the_health_and_safety_of_employees","Conducts_research_and_plans_and_directs_design_testing_and_manufacture_of_electrical_equipment","Studies_human_behavior_emotion_and_mental_processes_and_provides_counseling_and_therapy_for_individuals","Plans_and_directs_the_editorial_activities_of_various_publications","Creates_fiction_and_non-fiction_books_either_on_assignment_from_editors_or_independently","Counsels_clients_in_legal_matters;_using_interpretation_of_laws_and_rulings_to_advise_and_represent_businesses_and_individuals","Performs_examinations_diagnoses_medical_conditions_and_prescribes_treatment_for_individuals_suffering_from_injury_discomfort_or_disease","Assists_engineers_in_planning_design_and_development_by_applying_a_practical_knowledge_of_civil_mechanical_or_industrial_engineering","Aids_in_the_performance_of_essential_procedures_which_free_physicians_to_attend_to_more_specialized_aspects_of_their_work","Cashes_checks_makes_deposits_and_withdrawals_and_handles_a_variety_of_other_transactions_for_bank_customers","Creates_artwork_independently_usually_in_the_form_of_painting_drawing_sculpture_or_other_visual_mediums","Treats_and_rehabilitates_patients_suffering_from_cardiopulmonary_(heart_and_lung)_ailments_which_interfere_with_normal_breathing","Participates_in_the_assembly_of_books_or_magazines_for_commercial_printing_companies","Develops_photographic_film_makes_prints_and_slides_prepares_enlargements_and_retouches_photographs","Supervises_elementary_school_students_in_the_classroom_school_yard_and_in_the_cafeteria_operates_audio-visual_equipment_records_grades_and_prepares_educational_materials","Studies_diagnoses_and_treats_mental_emotional_and_behavioral_disorders","Installs_and_services_air-conditioning_and_furnace_systems_in_businesses_and_residences","Assists_physicians_in_administering_holistic_medical_care_and_treatment_to_assigned_patients_in_clinics_hospitals_public_health_centers_and_health_maintenance_organizations","Monitors_and_controls_processes_and_equipment_to_remove_harmful_waste_materials_from_sewage","Oversees_the_successful_and_profitable_operation_of_hotels_and_motels","Installs_services_cleans_tests_and_repairs_telephones_and_switchboard_systems","Maintains_inspects_and_repairs_industrial_machinery","Runs_and_maintains_the_equipment_used_to_transmit_radio_and_television_messages","Introduces_children_to_the_basics_of_mathematics_language_science_and_social_studies_and_assists_other_aspects_of_child_development","Diagnoses_ailments_and_performs_operations_to_repair_reconstruct_remove_or_replace_organs_limbs_and_bodily_systems","Installs_and_repairs_electronic_equipment_for_military_installations_manufacturers_and_businesses","Sells_insurance_and_advises_clients_about_amount_and_type_of_coverage_based_on_needs_and_circumstances","Greets_visitors_to_offices_answers_questions_and_refers_customers_to_appropriate_staff","Receives_payments_makes_change_and_provides_receipts_for_goods_sold","Negotiates_to_procure_accounts_and_supervises_advertising_campaigns_for_products_companies_and_organizations","Performs_maintenance_and_repairs_on_coin-operated_vending_and_amusement_machines","Plans_and_designs_spaces_to_be_constructed_or_remodeled_according_to_specifications_of_clients","Develops_assembles_and_tests_electrical_equipment_according_to_principles_of_electrical_engineering","Repairs_malfunctions_maintains_service_according_to_manufacturers'_schedules_and_sometimes_installs_computers_and_peripheral_equipment","Facilitates_the_purchase_and_sale_of_stocks_bonds_and_other_securities_for_individual_and_institutional_clients","Operates_industrial_trucks_and_tractors_to_move_products_and_raw_materials_for_manufacturing_firms","Helps_governmental_bodies_businesses_and_individuals_maintain_a_positive_image_with_the_public","Formulates_the_policies_and_directs_the_operations_of_private_and_publicly-held_companies","Supervises_the_work_of_employees_and_ensures_that_equipment_and_materials_are_being_used_properly_and_effectively","Adjusts_piano_strings_to_achieve_proper_musical_pitch","Builds_new_furniture_and_restores_worn_furniture_using_a_thorough_knowledge_of_fabrics_and_manufacturing_techniques","Installs_repairs_and_maintains_residential_and_commercial_phone_systems","Researches_methods_to_improve_quantity_and_quality_of_yields_from_farm_crops_and_livestock_and_attempts_to_find_practical_solutions_to_problems_in_agriculture","Typesetters_operate_keyboards_to_prepare_print_documents_while_compositors_lay_out_pages_check_proofs_and_make_corrections","Operates_an_aircraft_to_transport_passengers_and_cargo_to_appointed_destinations","Performs_major_and_routine_maintenance_on_a_variety_of_electrical_home_appliances","Repairs_and_maintains_business_machines","Prepares_bodies_for_burial_and_arranges_and_directs_funerals","Acts_as_intermediary_between_buyer_and_seller_in_real_estate_transactions_usually_by_being_the_prime_salesperson_of_a_property","Operates_manual_or_computerized_telephone_switchboards_and_assists_in_the_placement_of_local_and_long_distance_calls","Assists_doctors_and_registered_nurses_in_the_care_of_physically_or_mentally_ill_patients","Prepares_and_delivers_news_and_related_presentations_over_the_air_on_radio_and_television","Establishes_official_land_and_aquatic_boundaries_measures_construction_and_mining_sites_writes_technical_property_descriptions_for_deeds_and_leases_and_prepares_maps_and_charts","Monitors_the_movement_of_shipments_of_merchandise_into_and_out_of_places_of_business","Follows_design_instructions_operates_a_sewing_machine_to_join_reinforce_and_decorate_parts_of_garments_or_other_textiles","Constructs_and_repairs_precision_tools_gauges_holding_devices_and_stamping_tools_for_the_machine_industry","Protects_property_from_damages_incurred_by_theft_fire_and_vandalism","Assembles_parts_or_contributes_to_the_final_assembly_of_automobiles_and_trucks","Works_on_sub-assembly_or_final_assembly_of_products_such_as_machinery_electronic_equipment_or_aircraft","Operates_an_electric_diesel-electric_or_gas-turbine-_electric_locomotive_to_transport_passengers_and_freight","Operates_computerized_machines_in_the_manufacture_of_industrial_parts","Operates_machinery_in_manufacturing_plants_for_fabrication_of_industrial_parts","Takes_customer_orders_serves_food_and_drink_and_prepares_meal_checks","Mixes_and_serves_drinks_to_customers_of_a_tavern_restaurant_or_lounge","Cleans_offices_and_other_spaces_within_buildings_and_keeps_areas_in_good_condition","Builds_and_repairs_water_waste_disposal_drainage_and_gas_delivery_systems_for_residential_commercial_and_industrial_structures","Designs_articles_or_complete_lines_of_clothing_for_men_women_or_children","Uses_shutter-operated_cameras_and_photographic_emulsions_to_visually_portray_a_variety_of_subjects","Refurbishes_and_mends_worn_shoes_saddles_and_boots","Assists_practical_nurses_in_caring_for_patients._Nursing_homes_and_hospitals_are_the_primary_employers_of_nurse's_aides","Organizes_and_supervises_a_variety_of_leisure_activities_including_sports_arts_crafts_drama_singing_dancing_and_story_telling","Maps_layout_and_installs_and_repairs_electrical_wiring_and_fixtures","Tends_to_the_care_and_comfort_of_passengers_on_commercial_and_corporate_aircraft","Measures_cuts_and_installs_glass_in_residential_and_commercial_buildings","Cleanses_nuclear_power_plant_equipment_and_personnel_of_irradiated_material","Mends_cosmetic_and_structural_damage_to_car_truck_and_bus_chassis","Represents_wholesalers_who_distribute_products_to_stores_manufacturers_and_businesses","Operates_drilling_machines_to_place_holes_in_metal_or_nonmetal_pieces_according_to_predetermined_specifications","Performs_scheduled_inspections_maintenance_and_repairs_on_commercial_and_private_aircraft","Orders_merchandise_and_maintains_inventory_control_for_wholesale_and_retail_sales_firms","Drives_limousines_or_other_vehicles_to_transport_passengers_to_and_from_specified_locations","Manages_the_successful_operation_of_a_crop_livestock_dairy_or_poultry_farm","Provides_courteous_and_efficient_service_to_customers_in_retail_stores","Supervises_inmates'_activities_and_enforces_regulations_in_jails_prisons_and_other_correctional_facilities","Installs_and_prepares_surfaces_of_drywall_panels_for_commercial_or_residential_interiors","Lays_tile_or_carpets_in_homes_and_businesses","Entertains_informs_and_instructs_audiences_by_interpreting_dramatic_roles_on_stage_film_television_or_radio","Operates_monitors_and_repairs_power_plants_and_industrial_heating_cooling_and_ventilation_systems","Transports_passengers_according_to_a_specific_schedule_along_metropolitan_and_community_routes","Patrols_roads_and_highways_and_enforces_traffic_regulations_and_criminal_statutes","Cleans_the_plates_glasses_and_silverware_used_by_patrons_of_an_eating_establishment_and_the_pots_pans_and_cooking_utensils_used_by_chefs","Erects_structures_such_as_fireplaces_and_walls_with_brick_and_other_masonry_materials","May_perform_a_variety_of_services_including_cleaning_and_upkeep_for_residential_and_institutional_employers","Negotiates_contracts_with_clients_for_advertising_in_publications_and_on_radio_and_television","Operates_16-wheeled_tractor-trailer_to_transport_durable_and_perishable_goods_from_producers_to_consumers","Makes_arrangements_for_travel_according_to_the_specific_needs_of_individuals_and_businesses","Operates_one_or_more_machines_used_in_extractive_or_construction_work","Delivers_and_collects_mail_along_prearranged_rural_and_urban_routes","Assists_in_the_construction_repairing_and_remodeling_of_buildings_and_other_structures","Protects_individuals_and_saves_lives_and_property_from_the_ravages_of_fire","Selects_and_plays_records_or_tapes;_comments_on_areas_of_interest_to_a_particular_radio_audience","Provides_protection_against_crime_investigates_criminal_activity_and_works_with_the_public_on_crime-prevention_measures","Collects_refuse_on_a_designated_municipal_route_and_transports_trash_to_disposal_plants_or_landfill_areas","Instructs_performers_and_develops_and_interprets_routines_for_stage_and_other_presentations","Applies_the_plaster_finish_to_interior_walls_and_ceilings_and_cement_finish_and_stucco_to_exterior_surfaces","Prepares_meat_for_sale_to_distributors_supermarket_customers_and_other_consumers","Diagnoses_problems_with_automotive_vehicles_makes_repairs_and_performs_routine_maintenance","Directs_and_takes_part_in_activities_involved_in_the_raising_of_cattle_for_milk_production","Photographs_newsworthy_events_for_publication_in_newspapers_and_magazines","Cares_for_infants_and_toddlers_when_parents_are_at_work_or_are_unable_to_do_so_for_other_reasons","Constructs_installs_and_maintains_sheet_metal_products_for_home_commercial_and_industrial_use","Covers_newsworthy_events_for_newspapers_magazines_and_television_news_programs","Performs_any_number_of_tasks_involved_in_the_operation_of_ships_boats_barges_or_dredges","Loads_and_unloads_cargo_from_vessels_routes_cargo_to_proper_locations","Assists_construction_trade_workers_by_performing_a_wide_variety_of_tasks_requiring_physical_labor","Monitors_public_utility_meters_and_records_volume_of_consumption_by_customers","Prepares_surfaces_and_applies_paints_varnishes_and_finishes_to_the_interiors_and_exteriors_of_houses_and_other_structures","Joins_or_repairs_metal_surfaces_through_the_application_of_heat","Attends_to_situations_which_demand_immediate_medical_attention_such_as_automobile_accidents_heart_attacks_and_gunshot_wounds","Operates_a_taxi_cab_over_the_streets_and_roads_of_a_municipality_picking_up_and_dropping_off_passengers_by_request","Installs_roofs_on_new_buildings_performs_repairs_on_old_roofs_and_re-roofs_old_buildings","Fells_cuts_and_transports_timber_to_be_processed_into_lumber_paper_and_other_wood_products","Raises_the_steel_framework_of_buildings_bridges_and_other_structures","Performs_routine_physical_labor_and_maintenance_on_oil_rigs_and_pipelines_both_on_and_off_shore"]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th> <\/th>\n      <th>Index<\/th>\n      <th>Job_Title<\/th>\n      <th>Overall_Score<\/th>\n      <th>Average_Income(USD)<\/th>\n      <th>Work_Environment<\/th>\n      <th>Stress_Level<\/th>\n      <th>Stress_Category<\/th>\n      <th>Physical_Demand<\/th>\n      <th>Hiring_Potential<\/th>\n      <th>Description<\/th>\n    <\/tr>\n  <\/thead>\n<\/table>","options":{"autoWidth":true,"columnDefs":[{"width":"100%","targets":[1,3]},{"className":"dt-right","targets":[1,3,4,5,6,7,8,9]},{"orderable":false,"targets":0}],"order":[],"orderClasses":false}},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

- We have 200 observations (200 jobs) and 10 variables (information about the jobs). 

```r
str(job_data)
```

```
## 'data.frame':	200 obs. of  10 variables:
##  $ Index              : int  1 2 3 4 5 6 7 8 9 10 ...
##  $ Job_Title          : chr  "Software_Engineer" "Mathematician" "Actuary" "Statistician" ...
##  $ Overall_Score      : int  60 73 123 129 147 175 182 192 195 197 ...
##  $ Average_Income(USD): int  87140 94178 87204 73208 77153 85210 74278 63208 63144 67107 ...
##  $ Work_Environment   : num  150 89.7 179.4 89.5 90.8 ...
##  $ Stress_Level       : num  10.4 12.8 16 14.1 16.5 ...
##  $ Stress_Category    : int  1 1 1 1 1 1 1 1 0 1 ...
##  $ Physical_Demand    : num  5 3.97 3.97 3.95 5.08 6.98 4.98 5.09 7.43 7 ...
##  $ Hiring_Potential   : num  27.4 19.8 17 11.1 15.5 ...
##  $ Description        : chr  "Researches_designs_develops_and_maintains_software_systems_along_with_hardware_development_for_medical_scientif"| __truncated__ "Applies_mathematical_theories_and_formulas_to_teach_or_solve_problems_in_a_business_educational_or_industrial_climate" "Interprets_statistics_to_determine_probabilities_of_accidents_sickness_and_death_and_loss_of_property_from_thef"| __truncated__ "Tabulates_analyzes_and_interprets_the_numeric_results_of_experiments_and_surveys" ...
```

## Prepare the data

- Remove Index variable, which contains the rank-ordering of the jobs

```r
job_data$Index <- NULL
```

- Turn Stress_Category into a factor

```r
# turn Stress_Category into a factor
job_data$Stress_Category <- factor(job_data$Stress_Category)
```

- Remove some of the unneccessary *punction,* *contractions,* and *conjunctions* from the job description variable using `gsub` 

```r
# remove the underscores in the job descriptions using the gsub() function
job_data$Description <- gsub("_", " ", job_data$Description)

# remove the apostrophes and contractions
job_data$Description <- gsub("'", "", job_data$Description)

# remove all the "and", "the", and "for" because they aren't super relevant
job_data$Description <- gsub(" and ", " ", job_data$Description)
job_data$Description <- gsub(" the ", " ", job_data$Description)
job_data$Description <- gsub(" for ", " ", job_data$Description)
job_data$Description <- gsub(" from ", " ", job_data$Description)
job_data$Description <- gsub(" with ", " ", job_data$Description)
```


# Process text data for analysis
- Install `tm` package

```r
# install.packages("tm", repos = "http://cran.us.r-project.org")
# requires R V.3.3.1 +
library(tm)
```

- First step for text mining is to convert text features (text elements) into a corpus object, which is a collection of text documents.
- From homework: *Convert the textual JD meta-data into a corpus object.* 

```r
job_data_corpus <- Corpus(VectorSource(job_data$Description))  
## VectorSource() interprets each element of the vector x as a document
## A Corpus is unique because Corpus objects store text alongside metadata, like author, datetimestamp, id, language, etc.

print(job_data_corpus)
```

```
## <<SimpleCorpus>>
## Metadata:  corpus specific: 1, document level (indexed): 0
## Content:  documents: 200
```

- Each document represents a single description for a job. We have 200 jobs in our dataset, thus 200 documents corresponding to 200 job descriptions.

```r
inspect(job_data_corpus[1:3])
```

```
## <<SimpleCorpus>>
## Metadata:  corpus specific: 1, document level (indexed): 0
## Content:  documents: 3
## 
## [1] Researches designs develops maintains software systems along hardware development medical scientific industrial purposes
## [2] Applies mathematical theories formulas to teach or solve problems in a business educational or industrial climate       
## [3] Interprets statistics to determine probabilities of accidents sickness death loss of property theft natural disasters
```

- Use `tm_map()` function for cleaning the corpus document.
- From homework: *Triage some of the irrelevant punctuation and other symbols in the corpus document, change all text to lower case, etc.* 

```r
corpus_clean <- tm_map(job_data_corpus, tolower)  ## changed all the characters to lower case

corpus_clean <- tm_map(corpus_clean, removePunctuation) ## removed all punctuations

corpus_clean <- tm_map(corpus_clean, removeNumbers) ## remove all numbers 

corpus_clean <- tm_map(corpus_clean, stripWhitespace) ## remove all extra white spaces  (typically created by deleting punctuations)
```

- Inspect the corpus to make sure everything looks alright

```r
inspect(corpus_clean[21:27])
```

```
## <<SimpleCorpus>>
## Metadata:  corpus specific: 1, document level (indexed): 0
## Content:  documents: 7
## 
## [1] assesses hearing speech language disabilities provides treatment assists individuals communication disorders through diagnostic techniques
## [2] uses principles of physics mathematics to understand workings of universe                                                                 
## [3] loan officers help people apply loans this lets people do things like buy a house or a car or pay college                                 
## [4] plans drilling locations effective production methods optimal access to oil natural gas                                                   
## [5] assesses patients dietary needs plans menus instructs patients their families about proper nutritional care                               
## [6] transforms scientific technical information into readily understandable language                                                          
## [7] diagnoses visual disorders prescribes administers corrective rehabilitative treatments
```

- `DocumentTermMatrix()` function can successfully tokenize the job description into words. It can count frequent terms in each document in the corpus object.
- From homework: *Tokenize the job descriptions into words.*

```r
job_data_dtm <- DocumentTermMatrix(corpus_clean)
```

- Inspect to make sure DocumentTermMatrix looks correct

```r
head(job_data_dtm$dimnames$Terms)
```

```
## [1] "along"       "designs"     "development" "develops"    "hardware"   
## [6] "industrial"
```

```r
tail(job_data_dtm$dimnames$Terms)
```

```
## [1] "framework" "raises"    "steel"     "pipelines" "rigs"      "shore"
```

```r
inspect(job_data_dtm[,1:5])
```

```
## <<DocumentTermMatrix (documents: 200, terms: 5)>>
## Non-/sparse entries: 25/975
## Sparsity           : 98%
## Maximal term length: 11
## Weighting          : term frequency (tf)
## Sample             :
##     Terms
## Docs along designs development develops hardware
##   1      1       1           1        1        1
##   14     0       0           0        1        0
##   17     0       0           0        1        0
##   19     0       1           0        1        0
##   49     0       1           0        0        0
##   5      0       0           0        1        0
##   51     0       1           0        1        0
##   52     0       0           1        0        0
##   62     0       0           0        1        0
##   69     0       0           0        1        0
```

## Visualize Job Description Text

```r
library(plotly)
```


```r
# Frequency
freq <- sort(colSums(as.matrix(job_data_dtm)), decreasing=TRUE)
wf <- data.frame(word=names(freq), freq=freq)

# Plot Histogram
h1 <- subset(wf, freq>8)    %>%
        ggplot(aes(word, freq)) +
        geom_bar(stat="identity", fill="darkred", colour="darkblue") +
        theme(axis.text.x=element_text(angle=45, hjust=1)) + ggtitle("Frequency Histogram")
ggplotly(h1)
```

<!--html_preserve--><div id="htmlwidget-8158a2415799d956ec20" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-8158a2415799d956ec20">{"x":{"data":[{"orientation":"v","width":[0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999],"base":[0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],"x":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20],"y":[9,14,10,9,11,9,13,12,11,10,11,13,20,10,11,15,9,10,19,13],"text":["word: according<br />freq:  9","word: assists<br />freq: 14","word: businesses<br />freq: 10","word: commercial<br />freq:  9","word: develops<br />freq: 11","word: diagnoses<br />freq:  9","word: equipment<br />freq: 13","word: individuals<br />freq: 12","word: industrial<br />freq: 11","word: installs<br />freq: 10","word: maintains<br />freq: 11","word: operates<br />freq: 13","word: other<br />freq: 20","word: performs<br />freq: 10","word: plans<br />freq: 11","word: prepares<br />freq: 15","word: problems<br />freq:  9","word: products<br />freq: 10","word: repairs<br />freq: 19","word: studies<br />freq: 13"],"type":"bar","marker":{"autocolorscale":false,"color":"rgba(139,0,0,1)","line":{"width":1.88976377952756,"color":"rgba(0,0,139,1)"}},"showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":62.8209878231037,"l":37.2602739726027},"plot_bgcolor":"rgba(235,235,235,1)","paper_bgcolor":"rgba(255,255,255,1)","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"Frequency Histogram","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[0.4,20.6],"tickmode":"array","ticktext":["according","assists","businesses","commercial","develops","diagnoses","equipment","individuals","industrial","installs","maintains","operates","other","performs","plans","prepares","problems","products","repairs","studies"],"tickvals":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20],"categoryorder":"array","categoryarray":["according","assists","businesses","commercial","develops","diagnoses","equipment","individuals","industrial","installs","maintains","operates","other","performs","plans","prepares","problems","products","repairs","studies"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.65296803652968,"tickwidth":0.66417600664176,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-45,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(255,255,255,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"word","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-1,21],"tickmode":"array","ticktext":["0","5","10","15","20"],"tickvals":[0,5,10,15,20],"categoryorder":"array","categoryarray":["0","5","10","15","20"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.65296803652968,"tickwidth":0.66417600664176,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(255,255,255,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"freq","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":"rgba(255,255,255,1)","bordercolor":"transparent","borderwidth":1.88976377952756,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"1812b1aa33bb4":{"x":{},"y":{},"type":"bar"}},"cur_data":"1812b1aa33bb4","visdat":{"1812b1aa33bb4":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

- From homework: Examine the distributions of **Stress_Category** and **Hiring_Potential**.

```r
psych::describe(job_data$Hiring_Potential)
```

```
##    vars   n mean    sd median trimmed   mad    min   max range  skew
## X1    1 200 3.84 12.15   4.59    4.01 11.13 -40.76 37.05 77.81 -0.26
##    kurtosis   se
## X1     0.68 0.86
```

```r
table(job_data$Stress_Category)
```

```
## 
##  0  1  2  3  4  5 
## 12 85 64 24 13  2
```


```r
plot(job_data$Stress_Category, main = "Stress Category Frequency")
```

<img src="/project/textAnalysisJobSatisfaction_DSPA4/index_files/figure-html/unnamed-chunk-17-1.png" width="672" />

```r
q <- ggplot(job_data, aes(x=Hiring_Potential)) +
    geom_histogram(binwidth=.5, colour="black", fill="white") +
    geom_vline(aes(xintercept=mean(Hiring_Potential, na.rm=T)),   # Ignore NA values for mean
               color="red", linetype="dashed", size=1) + ggtitle("Hiring Potential Frequency")
ggplotly(q)
```

<!--html_preserve--><div id="htmlwidget-34d4895ff206e08f38ac" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-34d4895ff206e08f38ac">{"x":{"data":[{"orientation":"v","width":[0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5,0.5],"base":[0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],"x":[-41,-40.5,-40,-39.5,-39,-38.5,-38,-37.5,-37,-36.5,-36,-35.5,-35,-34.5,-34,-33.5,-33,-32.5,-32,-31.5,-31,-30.5,-30,-29.5,-29,-28.5,-28,-27.5,-27,-26.5,-26,-25.5,-25,-24.5,-24,-23.5,-23,-22.5,-22,-21.5,-21,-20.5,-20,-19.5,-19,-18.5,-18,-17.5,-17,-16.5,-16,-15.5,-15,-14.5,-14,-13.5,-13,-12.5,-12,-11.5,-11,-10.5,-10,-9.5,-9,-8.5,-8,-7.5,-7,-6.5,-6,-5.5,-5,-4.5,-4,-3.5,-3,-2.5,-2,-1.5,-1,-0.5,0,0.5,1,1.5,2,2.5,3,3.5,4,4.5,5,5.5,6,6.5,7,7.5,8,8.5,9,9.5,10,10.5,11,11.5,12,12.5,13,13.5,14,14.5,15,15.5,16,16.5,17,17.5,18,18.5,19,19.5,20,20.5,21,21.5,22,22.5,23,23.5,24,24.5,25,25.5,26,26.5,27,27.5,28,28.5,29,29.5,30,30.5,31,31.5,32,32.5,33,33.5,34,34.5,35,35.5,36,36.5,37],"y":[1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1,0,0,1,0,0,0,0,0,0,0,0,0,0,0,0,1,0,0,0,0,0,0,0,1,1,0,0,0,1,1,0,0,1,0,4,1,1,1,3,0,3,1,2,0,3,0,2,2,0,4,2,0,2,2,0,2,8,3,5,2,2,5,3,4,4,6,1,1,3,1,3,2,6,3,5,3,6,4,3,2,5,2,5,4,2,4,0,4,0,2,1,1,6,1,3,0,2,1,5,1,0,2,1,3,2,0,3,0,1,0,1,2,0,0,1,1,0,0,1,1,0,0,0,0,0,0,0,0,0,2,0,0,0,0,0,0,0,1],"text":["count: 1<br />Hiring_Potential: -41.0","count: 0<br />Hiring_Potential: -40.5","count: 0<br />Hiring_Potential: -40.0","count: 0<br />Hiring_Potential: -39.5","count: 0<br />Hiring_Potential: -39.0","count: 0<br />Hiring_Potential: -38.5","count: 0<br />Hiring_Potential: -38.0","count: 0<br />Hiring_Potential: -37.5","count: 0<br />Hiring_Potential: -37.0","count: 0<br />Hiring_Potential: -36.5","count: 0<br />Hiring_Potential: -36.0","count: 0<br />Hiring_Potential: -35.5","count: 0<br />Hiring_Potential: -35.0","count: 0<br />Hiring_Potential: -34.5","count: 0<br />Hiring_Potential: -34.0","count: 0<br />Hiring_Potential: -33.5","count: 0<br />Hiring_Potential: -33.0","count: 0<br />Hiring_Potential: -32.5","count: 1<br />Hiring_Potential: -32.0","count: 0<br />Hiring_Potential: -31.5","count: 0<br />Hiring_Potential: -31.0","count: 1<br />Hiring_Potential: -30.5","count: 0<br />Hiring_Potential: -30.0","count: 0<br />Hiring_Potential: -29.5","count: 0<br />Hiring_Potential: -29.0","count: 0<br />Hiring_Potential: -28.5","count: 0<br />Hiring_Potential: -28.0","count: 0<br />Hiring_Potential: -27.5","count: 0<br />Hiring_Potential: -27.0","count: 0<br />Hiring_Potential: -26.5","count: 0<br />Hiring_Potential: -26.0","count: 0<br />Hiring_Potential: -25.5","count: 0<br />Hiring_Potential: -25.0","count: 0<br />Hiring_Potential: -24.5","count: 1<br />Hiring_Potential: -24.0","count: 0<br />Hiring_Potential: -23.5","count: 0<br />Hiring_Potential: -23.0","count: 0<br />Hiring_Potential: -22.5","count: 0<br />Hiring_Potential: -22.0","count: 0<br />Hiring_Potential: -21.5","count: 0<br />Hiring_Potential: -21.0","count: 0<br />Hiring_Potential: -20.5","count: 1<br />Hiring_Potential: -20.0","count: 1<br />Hiring_Potential: -19.5","count: 0<br />Hiring_Potential: -19.0","count: 0<br />Hiring_Potential: -18.5","count: 0<br />Hiring_Potential: -18.0","count: 1<br />Hiring_Potential: -17.5","count: 1<br />Hiring_Potential: -17.0","count: 0<br />Hiring_Potential: -16.5","count: 0<br />Hiring_Potential: -16.0","count: 1<br />Hiring_Potential: -15.5","count: 0<br />Hiring_Potential: -15.0","count: 4<br />Hiring_Potential: -14.5","count: 1<br />Hiring_Potential: -14.0","count: 1<br />Hiring_Potential: -13.5","count: 1<br />Hiring_Potential: -13.0","count: 3<br />Hiring_Potential: -12.5","count: 0<br />Hiring_Potential: -12.0","count: 3<br />Hiring_Potential: -11.5","count: 1<br />Hiring_Potential: -11.0","count: 2<br />Hiring_Potential: -10.5","count: 0<br />Hiring_Potential: -10.0","count: 3<br />Hiring_Potential:  -9.5","count: 0<br />Hiring_Potential:  -9.0","count: 2<br />Hiring_Potential:  -8.5","count: 2<br />Hiring_Potential:  -8.0","count: 0<br />Hiring_Potential:  -7.5","count: 4<br />Hiring_Potential:  -7.0","count: 2<br />Hiring_Potential:  -6.5","count: 0<br />Hiring_Potential:  -6.0","count: 2<br />Hiring_Potential:  -5.5","count: 2<br />Hiring_Potential:  -5.0","count: 0<br />Hiring_Potential:  -4.5","count: 2<br />Hiring_Potential:  -4.0","count: 8<br />Hiring_Potential:  -3.5","count: 3<br />Hiring_Potential:  -3.0","count: 5<br />Hiring_Potential:  -2.5","count: 2<br />Hiring_Potential:  -2.0","count: 2<br />Hiring_Potential:  -1.5","count: 5<br />Hiring_Potential:  -1.0","count: 3<br />Hiring_Potential:  -0.5","count: 4<br />Hiring_Potential:   0.0","count: 4<br />Hiring_Potential:   0.5","count: 6<br />Hiring_Potential:   1.0","count: 1<br />Hiring_Potential:   1.5","count: 1<br />Hiring_Potential:   2.0","count: 3<br />Hiring_Potential:   2.5","count: 1<br />Hiring_Potential:   3.0","count: 3<br />Hiring_Potential:   3.5","count: 2<br />Hiring_Potential:   4.0","count: 6<br />Hiring_Potential:   4.5","count: 3<br />Hiring_Potential:   5.0","count: 5<br />Hiring_Potential:   5.5","count: 3<br />Hiring_Potential:   6.0","count: 6<br />Hiring_Potential:   6.5","count: 4<br />Hiring_Potential:   7.0","count: 3<br />Hiring_Potential:   7.5","count: 2<br />Hiring_Potential:   8.0","count: 5<br />Hiring_Potential:   8.5","count: 2<br />Hiring_Potential:   9.0","count: 5<br />Hiring_Potential:   9.5","count: 4<br />Hiring_Potential:  10.0","count: 2<br />Hiring_Potential:  10.5","count: 4<br />Hiring_Potential:  11.0","count: 0<br />Hiring_Potential:  11.5","count: 4<br />Hiring_Potential:  12.0","count: 0<br />Hiring_Potential:  12.5","count: 2<br />Hiring_Potential:  13.0","count: 1<br />Hiring_Potential:  13.5","count: 1<br />Hiring_Potential:  14.0","count: 6<br />Hiring_Potential:  14.5","count: 1<br />Hiring_Potential:  15.0","count: 3<br />Hiring_Potential:  15.5","count: 0<br />Hiring_Potential:  16.0","count: 2<br />Hiring_Potential:  16.5","count: 1<br />Hiring_Potential:  17.0","count: 5<br />Hiring_Potential:  17.5","count: 1<br />Hiring_Potential:  18.0","count: 0<br />Hiring_Potential:  18.5","count: 2<br />Hiring_Potential:  19.0","count: 1<br />Hiring_Potential:  19.5","count: 3<br />Hiring_Potential:  20.0","count: 2<br />Hiring_Potential:  20.5","count: 0<br />Hiring_Potential:  21.0","count: 3<br />Hiring_Potential:  21.5","count: 0<br />Hiring_Potential:  22.0","count: 1<br />Hiring_Potential:  22.5","count: 0<br />Hiring_Potential:  23.0","count: 1<br />Hiring_Potential:  23.5","count: 2<br />Hiring_Potential:  24.0","count: 0<br />Hiring_Potential:  24.5","count: 0<br />Hiring_Potential:  25.0","count: 1<br />Hiring_Potential:  25.5","count: 1<br />Hiring_Potential:  26.0","count: 0<br />Hiring_Potential:  26.5","count: 0<br />Hiring_Potential:  27.0","count: 1<br />Hiring_Potential:  27.5","count: 1<br />Hiring_Potential:  28.0","count: 0<br />Hiring_Potential:  28.5","count: 0<br />Hiring_Potential:  29.0","count: 0<br />Hiring_Potential:  29.5","count: 0<br />Hiring_Potential:  30.0","count: 0<br />Hiring_Potential:  30.5","count: 0<br />Hiring_Potential:  31.0","count: 0<br />Hiring_Potential:  31.5","count: 0<br />Hiring_Potential:  32.0","count: 0<br />Hiring_Potential:  32.5","count: 2<br />Hiring_Potential:  33.0","count: 0<br />Hiring_Potential:  33.5","count: 0<br />Hiring_Potential:  34.0","count: 0<br />Hiring_Potential:  34.5","count: 0<br />Hiring_Potential:  35.0","count: 0<br />Hiring_Potential:  35.5","count: 0<br />Hiring_Potential:  36.0","count: 0<br />Hiring_Potential:  36.5","count: 1<br />Hiring_Potential:  37.0"],"type":"bar","marker":{"autocolorscale":false,"color":"rgba(255,255,255,1)","line":{"width":1.88976377952756,"color":"rgba(0,0,0,1)"}},"showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735,3.83735],"y":[-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,-0.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4],"text":"mean(Hiring_Potential, na.rm = T): 3.83735","type":"scatter","mode":"lines","line":{"width":3.77952755905512,"color":"rgba(255,0,0,1)","dash":"dash"},"hoveron":"points","showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":40.1826484018265,"l":31.4155251141553},"plot_bgcolor":"rgba(235,235,235,1)","paper_bgcolor":"rgba(255,255,255,1)","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"Hiring Potential Frequency","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-45.175,41.175],"tickmode":"array","ticktext":["-40","-20","0","20","40"],"tickvals":[-40,-20,0,20,40],"categoryorder":"array","categoryarray":["-40","-20","0","20","40"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.65296803652968,"tickwidth":0.66417600664176,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(255,255,255,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"Hiring_Potential","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-0.4,8.4],"tickmode":"array","ticktext":["0","2","4","6","8"],"tickvals":[0,2,4,6,8],"categoryorder":"array","categoryarray":["0","2","4","6","8"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.65296803652968,"tickwidth":0.66417600664176,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(255,255,255,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"count","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":"rgba(255,255,255,1)","bordercolor":"transparent","borderwidth":1.88976377952756,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"1812b26a2f3f2":{"x":{},"type":"bar"},"1812b143da8af":{"xintercept":{}}},"cur_data":"1812b26a2f3f2","visdat":{"1812b26a2f3f2":["function (y) ","x"],"1812b143da8af":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

```r
p <- ggplot(data=job_data, aes(x=Stress_Category, y = Hiring_Potential)) +
    geom_bar(stat="identity") + ggtitle("Hiring Potential by Stress Category") 

ggplotly(p)
```

<!--html_preserve--><div id="htmlwidget-27537f9bc09c7c5cba4b" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-27537f9bc09c7c5cba4b">{"x":{"data":[{"orientation":"v","width":[0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9],"base":[-8,-10.89,-34.95,-55.13,-69.44,-79.97,-94.34,-108.84,-0.6,-7.84,-13.09,-16.68,-33.64,-35.69,-35.86,-36.67,-40.33,-41.02,-50.3,-53.98,-63.31,-66.84,-78.2,-86.06,-89.77,-96.3,-108.65,-119.13,-130.56,-138.93,-156.61,-169.39,-176.17,-188.79,-193.79,-196.72,-203.47,-235.35,-246.57,-287.33,-300.95,-303.42,-308.49,-313.99,-1.61,-4.26,-7.8,-10.38,-13.06,-14.3,-15.55,-19.13,-19.29,-30.54,-36.94,-38.94,-39.86,-54,-84.29,-100.03,-119.6,-7.09,-11.02,-13.93,-18.1,-21.58,-30.9,-43.63,-0.95,-3.54,-3.98,-4.82,-13.35,-27.6,0,21.44,35.73,42.21,0,27.4,47.18,64.22,75.3,90.83,102.93,114.71,125.79,158.86,178.49,195.23,218.76,230.72,234.36,242.92,267.14,284.69,302.12,307.51,317.98,322.25,326.95,346.88,353.36,363.36,380.92,402.5,426.6,441.3,447.95,452.33,465.09,477.23,505.27,514.98,520.79,530.28,538.71,571.69,587.38,593.04,603.18,604.37,610.82,623.67,639.15,639.65,643,0,8.3,22.63,45.02,51.24,58.43,67.29,73.81,74.81,92.46,111.57,126.68,141.42,167.17,168.53,194.43,208.69,216.23,230.67,237.46,246.25,266.9,273.25,282.67,300.21,301.28,309.6,313.78,324.81,343.71,357.68,361.5,364.15,375.04,385.42,392.28,393.46,398.65,403.87,404.4,407.79,408.46,410.74,411.43,420.17,428.31,428.31,0,5.09,26.38,36.49,73.54,93.68,114.26,123.68,129.61,134.31,144.41,160.98,166.43,180.06,181.14,190.64,197.75,0,2.6,5.44,13.15,21,23.09,28.36,0,5.53],"x":[1,1,1,1,1,1,1,1,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,4,4,4,4,4,4,4,5,5,5,5,5,5,1,1,1,1,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,5,5,5,5,5,5,5,6,6],"y":[8,2.89,24.06,20.18,14.31,10.53,14.37,14.5,0.6,7.24,5.25,3.59,16.96,2.05,0.170000000000002,0.810000000000002,3.66,0.690000000000005,9.27999999999999,3.68,9.33000000000001,3.53,11.36,7.86,3.70999999999999,6.53,12.35,10.48,11.43,8.37,17.68,12.78,6.78,12.62,5,2.93000000000001,6.75,31.88,11.22,40.76,13.62,2.47000000000003,5.06999999999999,5.5,1.61,2.65,3.54,2.58,2.68,1.24,1.25,3.58,0.16,11.25,6.4,2,0.920000000000002,14.14,30.29,15.74,19.57,7.09,3.93,2.91,4.17,3.48,9.32,12.73,0.95,2.59,0.44,0.839999999999999,8.53,14.25,21.44,14.29,6.48,7.69,27.4,19.78,17.04,11.08,15.53,12.1,11.78,11.08,33.07,19.63,16.74,23.53,11.96,3.63999999999999,8.56,24.22,17.55,17.43,5.38999999999999,10.47,4.26999999999998,4.69999999999999,19.93,6.48000000000002,10,17.56,21.58,24.1,14.7,6.64999999999998,4.38,12.76,12.14,28.04,9.71000000000004,5.80999999999995,9.49000000000001,8.43000000000006,32.9799999999999,15.6900000000001,5.65999999999997,10.14,1.19000000000005,6.44999999999993,12.85,15.48,0.5,3.35000000000002,0.789999999999964,8.3,14.33,22.39,6.22,7.19,8.86000000000001,6.52,1,17.65,19.11,15.11,14.74,25.75,1.36000000000001,25.9,14.26,7.53999999999999,14.44,6.79000000000002,8.78999999999999,20.65,6.35000000000002,9.42000000000002,17.54,1.06999999999999,8.31999999999999,4.18000000000001,11.03,18.9,13.97,3.81999999999999,2.64999999999998,10.89,10.3800000000001,6.85999999999996,1.18000000000001,5.19,5.22000000000003,0.529999999999973,3.39000000000004,0.669999999999959,2.28000000000003,0.689999999999998,8.74000000000001,8.13999999999999,0,4.5,5.09,21.29,10.11,37.05,20.14,20.58,9.42,5.92999999999999,4.70000000000002,10.1,16.57,5.45000000000002,13.63,1.07999999999998,9.5,7.11000000000001,4.68000000000001,2.6,2.84,7.71,7.85,2.09,5.27,0.0899999999999999,5.53,18.22],"text":["Stress_Category: 0<br />Hiring_Potential:  8.00","Stress_Category: 0<br />Hiring_Potential:  2.89","Stress_Category: 0<br />Hiring_Potential: 24.06","Stress_Category: 0<br />Hiring_Potential: 20.18","Stress_Category: 0<br />Hiring_Potential: 14.31","Stress_Category: 0<br />Hiring_Potential: 10.53","Stress_Category: 0<br />Hiring_Potential: 14.37","Stress_Category: 0<br />Hiring_Potential: 14.50","Stress_Category: 1<br />Hiring_Potential:  0.60","Stress_Category: 1<br />Hiring_Potential:  7.24","Stress_Category: 1<br />Hiring_Potential:  5.25","Stress_Category: 1<br />Hiring_Potential:  3.59","Stress_Category: 1<br />Hiring_Potential: 16.96","Stress_Category: 1<br />Hiring_Potential:  2.05","Stress_Category: 1<br />Hiring_Potential:  0.17","Stress_Category: 1<br />Hiring_Potential:  0.81","Stress_Category: 1<br />Hiring_Potential:  3.66","Stress_Category: 1<br />Hiring_Potential:  0.69","Stress_Category: 1<br />Hiring_Potential:  9.28","Stress_Category: 1<br />Hiring_Potential:  3.68","Stress_Category: 1<br />Hiring_Potential:  9.33","Stress_Category: 1<br />Hiring_Potential:  3.53","Stress_Category: 1<br />Hiring_Potential: 11.36","Stress_Category: 1<br />Hiring_Potential:  7.86","Stress_Category: 1<br />Hiring_Potential:  3.71","Stress_Category: 1<br />Hiring_Potential:  6.53","Stress_Category: 1<br />Hiring_Potential: 12.35","Stress_Category: 1<br />Hiring_Potential: 10.48","Stress_Category: 1<br />Hiring_Potential: 11.43","Stress_Category: 1<br />Hiring_Potential:  8.37","Stress_Category: 1<br />Hiring_Potential: 17.68","Stress_Category: 1<br />Hiring_Potential: 12.78","Stress_Category: 1<br />Hiring_Potential:  6.78","Stress_Category: 1<br />Hiring_Potential: 12.62","Stress_Category: 1<br />Hiring_Potential:  5.00","Stress_Category: 1<br />Hiring_Potential:  2.93","Stress_Category: 1<br />Hiring_Potential:  6.75","Stress_Category: 1<br />Hiring_Potential: 31.88","Stress_Category: 1<br />Hiring_Potential: 11.22","Stress_Category: 1<br />Hiring_Potential: 40.76","Stress_Category: 1<br />Hiring_Potential: 13.62","Stress_Category: 1<br />Hiring_Potential:  2.47","Stress_Category: 1<br />Hiring_Potential:  5.07","Stress_Category: 1<br />Hiring_Potential:  5.50","Stress_Category: 2<br />Hiring_Potential:  1.61","Stress_Category: 2<br />Hiring_Potential:  2.65","Stress_Category: 2<br />Hiring_Potential:  3.54","Stress_Category: 2<br />Hiring_Potential:  2.58","Stress_Category: 2<br />Hiring_Potential:  2.68","Stress_Category: 2<br />Hiring_Potential:  1.24","Stress_Category: 2<br />Hiring_Potential:  1.25","Stress_Category: 2<br />Hiring_Potential:  3.58","Stress_Category: 2<br />Hiring_Potential:  0.16","Stress_Category: 2<br />Hiring_Potential: 11.25","Stress_Category: 2<br />Hiring_Potential:  6.40","Stress_Category: 2<br />Hiring_Potential:  2.00","Stress_Category: 2<br />Hiring_Potential:  0.92","Stress_Category: 2<br />Hiring_Potential: 14.14","Stress_Category: 2<br />Hiring_Potential: 30.29","Stress_Category: 2<br />Hiring_Potential: 15.74","Stress_Category: 2<br />Hiring_Potential: 19.57","Stress_Category: 3<br />Hiring_Potential:  7.09","Stress_Category: 3<br />Hiring_Potential:  3.93","Stress_Category: 3<br />Hiring_Potential:  2.91","Stress_Category: 3<br />Hiring_Potential:  4.17","Stress_Category: 3<br />Hiring_Potential:  3.48","Stress_Category: 3<br />Hiring_Potential:  9.32","Stress_Category: 3<br />Hiring_Potential: 12.73","Stress_Category: 4<br />Hiring_Potential:  0.95","Stress_Category: 4<br />Hiring_Potential:  2.59","Stress_Category: 4<br />Hiring_Potential:  0.44","Stress_Category: 4<br />Hiring_Potential:  0.84","Stress_Category: 4<br />Hiring_Potential:  8.53","Stress_Category: 4<br />Hiring_Potential: 14.25","Stress_Category: 0<br />Hiring_Potential: 21.44","Stress_Category: 0<br />Hiring_Potential: 14.29","Stress_Category: 0<br />Hiring_Potential:  6.48","Stress_Category: 0<br />Hiring_Potential:  7.69","Stress_Category: 1<br />Hiring_Potential: 27.40","Stress_Category: 1<br />Hiring_Potential: 19.78","Stress_Category: 1<br />Hiring_Potential: 17.04","Stress_Category: 1<br />Hiring_Potential: 11.08","Stress_Category: 1<br />Hiring_Potential: 15.53","Stress_Category: 1<br />Hiring_Potential: 12.10","Stress_Category: 1<br />Hiring_Potential: 11.78","Stress_Category: 1<br />Hiring_Potential: 11.08","Stress_Category: 1<br />Hiring_Potential: 33.07","Stress_Category: 1<br />Hiring_Potential: 19.63","Stress_Category: 1<br />Hiring_Potential: 16.74","Stress_Category: 1<br />Hiring_Potential: 23.53","Stress_Category: 1<br />Hiring_Potential: 11.96","Stress_Category: 1<br />Hiring_Potential:  3.64","Stress_Category: 1<br />Hiring_Potential:  8.56","Stress_Category: 1<br />Hiring_Potential: 24.22","Stress_Category: 1<br />Hiring_Potential: 17.55","Stress_Category: 1<br />Hiring_Potential: 17.43","Stress_Category: 1<br />Hiring_Potential:  5.39","Stress_Category: 1<br />Hiring_Potential: 10.47","Stress_Category: 1<br />Hiring_Potential:  4.27","Stress_Category: 1<br />Hiring_Potential:  4.70","Stress_Category: 1<br />Hiring_Potential: 19.93","Stress_Category: 1<br />Hiring_Potential:  6.48","Stress_Category: 1<br />Hiring_Potential: 10.00","Stress_Category: 1<br />Hiring_Potential: 17.56","Stress_Category: 1<br />Hiring_Potential: 21.58","Stress_Category: 1<br />Hiring_Potential: 24.10","Stress_Category: 1<br />Hiring_Potential: 14.70","Stress_Category: 1<br />Hiring_Potential:  6.65","Stress_Category: 1<br />Hiring_Potential:  4.38","Stress_Category: 1<br />Hiring_Potential: 12.76","Stress_Category: 1<br />Hiring_Potential: 12.14","Stress_Category: 1<br />Hiring_Potential: 28.04","Stress_Category: 1<br />Hiring_Potential:  9.71","Stress_Category: 1<br />Hiring_Potential:  5.81","Stress_Category: 1<br />Hiring_Potential:  9.49","Stress_Category: 1<br />Hiring_Potential:  8.43","Stress_Category: 1<br />Hiring_Potential: 32.98","Stress_Category: 1<br />Hiring_Potential: 15.69","Stress_Category: 1<br />Hiring_Potential:  5.66","Stress_Category: 1<br />Hiring_Potential: 10.14","Stress_Category: 1<br />Hiring_Potential:  1.19","Stress_Category: 1<br />Hiring_Potential:  6.45","Stress_Category: 1<br />Hiring_Potential: 12.85","Stress_Category: 1<br />Hiring_Potential: 15.48","Stress_Category: 1<br />Hiring_Potential:  0.50","Stress_Category: 1<br />Hiring_Potential:  3.35","Stress_Category: 1<br />Hiring_Potential:  0.79","Stress_Category: 2<br />Hiring_Potential:  8.30","Stress_Category: 2<br />Hiring_Potential: 14.33","Stress_Category: 2<br />Hiring_Potential: 22.39","Stress_Category: 2<br />Hiring_Potential:  6.22","Stress_Category: 2<br />Hiring_Potential:  7.19","Stress_Category: 2<br />Hiring_Potential:  8.86","Stress_Category: 2<br />Hiring_Potential:  6.52","Stress_Category: 2<br />Hiring_Potential:  1.00","Stress_Category: 2<br />Hiring_Potential: 17.65","Stress_Category: 2<br />Hiring_Potential: 19.11","Stress_Category: 2<br />Hiring_Potential: 15.11","Stress_Category: 2<br />Hiring_Potential: 14.74","Stress_Category: 2<br />Hiring_Potential: 25.75","Stress_Category: 2<br />Hiring_Potential:  1.36","Stress_Category: 2<br />Hiring_Potential: 25.90","Stress_Category: 2<br />Hiring_Potential: 14.26","Stress_Category: 2<br />Hiring_Potential:  7.54","Stress_Category: 2<br />Hiring_Potential: 14.44","Stress_Category: 2<br />Hiring_Potential:  6.79","Stress_Category: 2<br />Hiring_Potential:  8.79","Stress_Category: 2<br />Hiring_Potential: 20.65","Stress_Category: 2<br />Hiring_Potential:  6.35","Stress_Category: 2<br />Hiring_Potential:  9.42","Stress_Category: 2<br />Hiring_Potential: 17.54","Stress_Category: 2<br />Hiring_Potential:  1.07","Stress_Category: 2<br />Hiring_Potential:  8.32","Stress_Category: 2<br />Hiring_Potential:  4.18","Stress_Category: 2<br />Hiring_Potential: 11.03","Stress_Category: 2<br />Hiring_Potential: 18.90","Stress_Category: 2<br />Hiring_Potential: 13.97","Stress_Category: 2<br />Hiring_Potential:  3.82","Stress_Category: 2<br />Hiring_Potential:  2.65","Stress_Category: 2<br />Hiring_Potential: 10.89","Stress_Category: 2<br />Hiring_Potential: 10.38","Stress_Category: 2<br />Hiring_Potential:  6.86","Stress_Category: 2<br />Hiring_Potential:  1.18","Stress_Category: 2<br />Hiring_Potential:  5.19","Stress_Category: 2<br />Hiring_Potential:  5.22","Stress_Category: 2<br />Hiring_Potential:  0.53","Stress_Category: 2<br />Hiring_Potential:  3.39","Stress_Category: 2<br />Hiring_Potential:  0.67","Stress_Category: 2<br />Hiring_Potential:  2.28","Stress_Category: 2<br />Hiring_Potential:  0.69","Stress_Category: 2<br />Hiring_Potential:  8.74","Stress_Category: 2<br />Hiring_Potential:  8.14","Stress_Category: 2<br />Hiring_Potential:  0.00","Stress_Category: 2<br />Hiring_Potential:  4.50","Stress_Category: 3<br />Hiring_Potential:  5.09","Stress_Category: 3<br />Hiring_Potential: 21.29","Stress_Category: 3<br />Hiring_Potential: 10.11","Stress_Category: 3<br />Hiring_Potential: 37.05","Stress_Category: 3<br />Hiring_Potential: 20.14","Stress_Category: 3<br />Hiring_Potential: 20.58","Stress_Category: 3<br />Hiring_Potential:  9.42","Stress_Category: 3<br />Hiring_Potential:  5.93","Stress_Category: 3<br />Hiring_Potential:  4.70","Stress_Category: 3<br />Hiring_Potential: 10.10","Stress_Category: 3<br />Hiring_Potential: 16.57","Stress_Category: 3<br />Hiring_Potential:  5.45","Stress_Category: 3<br />Hiring_Potential: 13.63","Stress_Category: 3<br />Hiring_Potential:  1.08","Stress_Category: 3<br />Hiring_Potential:  9.50","Stress_Category: 3<br />Hiring_Potential:  7.11","Stress_Category: 3<br />Hiring_Potential:  4.68","Stress_Category: 4<br />Hiring_Potential:  2.60","Stress_Category: 4<br />Hiring_Potential:  2.84","Stress_Category: 4<br />Hiring_Potential:  7.71","Stress_Category: 4<br />Hiring_Potential:  7.85","Stress_Category: 4<br />Hiring_Potential:  2.09","Stress_Category: 4<br />Hiring_Potential:  5.27","Stress_Category: 4<br />Hiring_Potential:  0.09","Stress_Category: 5<br />Hiring_Potential:  5.53","Stress_Category: 5<br />Hiring_Potential: 18.22"],"type":"bar","marker":{"autocolorscale":false,"color":"rgba(89,89,89,1)","line":{"width":1.88976377952756,"color":"transparent"}},"showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":40.1826484018265,"l":48.9497716894977},"plot_bgcolor":"rgba(235,235,235,1)","paper_bgcolor":"rgba(255,255,255,1)","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"Hiring Potential by Stress Category","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[0.4,6.6],"tickmode":"array","ticktext":["0","1","2","3","4","5"],"tickvals":[1,2,3,4,5,6],"categoryorder":"array","categoryarray":["0","1","2","3","4","5"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.65296803652968,"tickwidth":0.66417600664176,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(255,255,255,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"Stress_Category","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-361.879,691.679],"tickmode":"array","ticktext":["-250","0","250","500"],"tickvals":[-250,0,250,500],"categoryorder":"array","categoryarray":["-250","0","250","500"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.65296803652968,"tickwidth":0.66417600664176,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(255,255,255,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"Hiring_Potential","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":"rgba(255,255,255,1)","bordercolor":"transparent","borderwidth":1.88976377952756,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"1812b6e59611a":{"x":{},"y":{},"type":"bar"}},"cur_data":"1812b6e59611a","visdat":{"1812b6e59611a":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->


```r
h2 <- ggplot(data=job_data, aes(x=Stress_Category, y = Hiring_Potential, fill = Stress_Category)) + geom_boxplot() + ggtitle("Boxplot of Hiring Potential by Stress Category") + guides(fill=FALSE)

ggplotly(h2)
```

<!--html_preserve--><div id="htmlwidget-13c6f9a610f0e0145586" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-13c6f9a610f0e0145586">{"x":{"data":[{"x":[1,1,1,1,1,1,1,1,1,1,1,1],"y":[-10.53,-24.06,-14.5,6.48,7.69,-20.18,-14.37,-2.89,-8,-14.31,21.44,14.29],"hoverinfo":"y","type":"box","fillcolor":"rgba(248,118,109,1)","marker":{"opacity":null,"outliercolor":"rgba(0,0,0,1)","line":{"width":1.88976377952756,"color":"rgba(0,0,0,1)"},"size":5.66929133858268},"line":{"color":"rgba(51,51,51,1)","width":1.88976377952756},"name":"0","legendgroup":"0","showlegend":true,"xaxis":"x","yaxis":"y","frame":null},{"x":[2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2,2],"y":[27.4,19.78,17.04,11.08,15.53,12.1,11.78,11.08,-5.07,33.07,19.63,16.74,23.53,11.96,3.64,8.56,24.22,17.55,-9.28,-0.6,17.43,-3.68,5.39,10.47,4.27,4.7,19.93,-7.24,6.48,10,17.56,21.58,9.71,24.1,5.81,14.7,6.45,6.65,4.38,-12.35,0.79,12.76,12.14,9.49,28.04,-5,-2.47,-3.59,-5.5,-5.25,-0.81,-12.78,3.35,15.69,15.48,-2.05,-11.36,-0.69,10.14,-2.93,1.19,12.85,8.43,-6.53,32.98,-16.96,-31.88,-3.53,-10.48,-11.43,5.66,-12.62,-0.17,-8.37,-6.75,-3.71,-17.68,-7.86,0.5,-3.66,-6.78,-13.62,-40.76,-11.22,-9.33],"hoverinfo":"y","type":"box","fillcolor":"rgba(183,159,0,1)","marker":{"opacity":null,"outliercolor":"rgba(0,0,0,1)","line":{"width":1.88976377952756,"color":"rgba(0,0,0,1)"},"size":5.66929133858268},"line":{"color":"rgba(51,51,51,1)","width":1.88976377952756},"name":"1","legendgroup":"1","showlegend":true,"xaxis":"x","yaxis":"y","frame":null},{"x":[3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3,3],"y":[7.19,22.39,14.74,14.26,14.33,25.9,-2.65,17.65,19.11,-1.61,-1.24,8.86,-2.58,0.67,7.54,6.52,14.44,1.18,-2,4.18,-2.68,10.89,-3.54,8.79,1.07,20.65,11.03,3.39,25.75,6.35,-1.25,13.97,6.22,1,6.79,17.54,-3.58,15.11,4.5,6.86,-11.25,8.3,8.32,-6.4,1.36,3.82,2.65,9.42,0,10.38,-0.92,-0.16,-15.74,-14.14,-19.57,8.74,5.22,0.53,8.14,5.19,-30.29,18.9,0.69,2.28],"hoverinfo":"y","type":"box","fillcolor":"rgba(0,186,56,1)","marker":{"opacity":null,"outliercolor":"rgba(0,0,0,1)","line":{"width":1.88976377952756,"color":"rgba(0,0,0,1)"},"size":5.66929133858268},"line":{"color":"rgba(51,51,51,1)","width":1.88976377952756},"name":"2","legendgroup":"2","showlegend":true,"xaxis":"x","yaxis":"y","frame":null},{"x":[4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4],"y":[1.08,4.7,9.42,5.93,5.45,10.1,37.05,16.57,-2.91,10.11,20.58,-3.93,20.14,-3.48,-4.17,21.29,7.11,-7.09,-9.32,5.09,9.5,-12.73,4.68,13.63],"hoverinfo":"y","type":"box","fillcolor":"rgba(0,191,196,1)","marker":{"opacity":null,"outliercolor":"rgba(0,0,0,1)","line":{"width":1.88976377952756,"color":"rgba(0,0,0,1)"},"size":5.66929133858268},"line":{"color":"rgba(51,51,51,1)","width":1.88976377952756},"name":"3","legendgroup":"3","showlegend":true,"xaxis":"x","yaxis":"y","frame":null},{"x":[5,5,5,5,5,5,5,5,5,5,5,5,5],"y":[-8.53,2.84,-0.44,-14.25,2.09,-0.84,7.71,2.6,-2.59,7.85,5.27,0.09,-0.95],"hoverinfo":"y","type":"box","fillcolor":"rgba(97,156,255,1)","marker":{"opacity":null,"outliercolor":"rgba(0,0,0,1)","line":{"width":1.88976377952756,"color":"rgba(0,0,0,1)"},"size":5.66929133858268},"line":{"color":"rgba(51,51,51,1)","width":1.88976377952756},"name":"4","legendgroup":"4","showlegend":true,"xaxis":"x","yaxis":"y","frame":null},{"x":[6,6],"y":[18.22,5.53],"hoverinfo":"y","type":"box","fillcolor":"rgba(245,100,227,1)","marker":{"opacity":null,"outliercolor":"rgba(0,0,0,1)","line":{"width":1.88976377952756,"color":"rgba(0,0,0,1)"},"size":5.66929133858268},"line":{"color":"rgba(51,51,51,1)","width":1.88976377952756},"name":"5","legendgroup":"5","showlegend":true,"xaxis":"x","yaxis":"y","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":40.1826484018265,"l":43.1050228310502},"plot_bgcolor":"rgba(235,235,235,1)","paper_bgcolor":"rgba(255,255,255,1)","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"Boxplot of Hiring Potential by Stress Category","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[0.4,6.6],"tickmode":"array","ticktext":["0","1","2","3","4","5"],"tickvals":[1,2,3,4,5,6],"categoryorder":"array","categoryarray":["0","1","2","3","4","5"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.65296803652968,"tickwidth":0.66417600664176,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(255,255,255,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"Stress_Category","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-44.6505,40.9405],"tickmode":"array","ticktext":["-40","-20","0","20","40"],"tickvals":[-40,-20,0,20,40],"categoryorder":"array","categoryarray":["-40","-20","0","20","40"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.65296803652968,"tickwidth":0.66417600664176,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(255,255,255,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"Hiring_Potential","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":true,"legend":{"bgcolor":"rgba(255,255,255,1)","bordercolor":"transparent","borderwidth":1.88976377952756,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895},"y":1},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"1812b35f95520":{"x":{},"y":{},"fill":{},"type":"box"}},"cur_data":"1812b35f95520","visdat":{"1812b35f95520":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->



```r
library(ggplot2)
g <- ggplot(job_data) + geom_point(aes(x=Hiring_Potential, y=Work_Environment, colour=Stress_Category))
library(plotly)
ggplotly(g)
```

<!--html_preserve--><div id="htmlwidget-d784436566c3a74c77fd" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-d784436566c3a74c77fd">{"x":{"data":[{"x":[21.44,14.29,6.48,-8,-2.89,7.69,-24.06,-20.18,-14.31,-10.53,-14.37,-14.5],"y":[463.43,454.3,165.2,316.4,234.7,575.12,359.2,404.1,502.26,704.31,639.45,639.45],"text":["Hiring_Potential:  21.44<br />Work_Environment:  463.43<br />Stress_Category: 0","Hiring_Potential:  14.29<br />Work_Environment:  454.30<br />Stress_Category: 0","Hiring_Potential:   6.48<br />Work_Environment:  165.20<br />Stress_Category: 0","Hiring_Potential:  -8.00<br />Work_Environment:  316.40<br />Stress_Category: 0","Hiring_Potential:  -2.89<br />Work_Environment:  234.70<br />Stress_Category: 0","Hiring_Potential:   7.69<br />Work_Environment:  575.12<br />Stress_Category: 0","Hiring_Potential: -24.06<br />Work_Environment:  359.20<br />Stress_Category: 0","Hiring_Potential: -20.18<br />Work_Environment:  404.10<br />Stress_Category: 0","Hiring_Potential: -14.31<br />Work_Environment:  502.26<br />Stress_Category: 0","Hiring_Potential: -10.53<br />Work_Environment:  704.31<br />Stress_Category: 0","Hiring_Potential: -14.37<br />Work_Environment:  639.45<br />Stress_Category: 0","Hiring_Potential: -14.50<br />Work_Environment:  639.45<br />Stress_Category: 0"],"type":"scatter","mode":"markers","marker":{"autocolorscale":false,"color":"rgba(248,118,109,1)","opacity":1,"size":5.66929133858268,"symbol":"circle","line":{"width":1.88976377952756,"color":"rgba(248,118,109,1)"}},"hoveron":"points","name":"0","legendgroup":"0","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[27.4,19.78,17.04,11.08,15.53,12.1,11.78,11.08,33.07,19.63,16.74,23.53,11.96,3.64,8.56,24.22,17.55,-0.6,17.43,5.39,10.47,4.27,4.7,19.93,-7.24,6.48,10,17.56,21.58,24.1,14.7,6.65,4.38,12.76,12.14,28.04,9.71,5.81,-5.25,9.49,-3.59,8.43,32.98,-16.96,15.69,-2.05,5.66,10.14,1.19,6.45,-0.17,12.85,-0.81,15.48,-3.66,-0.69,-9.28,-3.68,-9.33,-3.53,-11.36,-7.86,-3.71,-6.53,0.5,-12.35,-10.48,-11.43,-8.37,-17.68,-12.78,3.35,-6.78,-12.62,-5,-2.93,-6.75,-31.88,-11.22,-40.76,0.79,-13.62,-2.47,-5.07,-5.5],"y":[150,89.72,179.44,89.52,90.78,179.64,314.37,136.41,593.25,322.24,276.78,263.82,269.46,225,361.44,505.56,381.06,227.35,463.43,390.56,322.42,334.4,221.25,538.34,265.08,385.2,371.7,361.44,587.28,318.29,788.64,230.85,204.4,404.1,449.1,716.21,556.4,359.28,228.1,783.04,222.95,371.7,978.8,160.64,581.14,449.1,356.72,673.65,354,538.92,642.56,884.73,546.78,734.24,657.16,422.46,351.76,657.16,595.36,469.4,610.22,454.5,563.28,504.36,494.01,440.9,610.22,611.91,719.82,520.32,740.55,812.06,753.1,756.8,687.42,796.74,489.6,597.24,669.9,728.32,973.94,796.86,868.32,682.02,987.8],"text":["Hiring_Potential:  27.40<br />Work_Environment:  150.00<br />Stress_Category: 1","Hiring_Potential:  19.78<br />Work_Environment:   89.72<br />Stress_Category: 1","Hiring_Potential:  17.04<br />Work_Environment:  179.44<br />Stress_Category: 1","Hiring_Potential:  11.08<br />Work_Environment:   89.52<br />Stress_Category: 1","Hiring_Potential:  15.53<br />Work_Environment:   90.78<br />Stress_Category: 1","Hiring_Potential:  12.10<br />Work_Environment:  179.64<br />Stress_Category: 1","Hiring_Potential:  11.78<br />Work_Environment:  314.37<br />Stress_Category: 1","Hiring_Potential:  11.08<br />Work_Environment:  136.41<br />Stress_Category: 1","Hiring_Potential:  33.07<br />Work_Environment:  593.25<br />Stress_Category: 1","Hiring_Potential:  19.63<br />Work_Environment:  322.24<br />Stress_Category: 1","Hiring_Potential:  16.74<br />Work_Environment:  276.78<br />Stress_Category: 1","Hiring_Potential:  23.53<br />Work_Environment:  263.82<br />Stress_Category: 1","Hiring_Potential:  11.96<br />Work_Environment:  269.46<br />Stress_Category: 1","Hiring_Potential:   3.64<br />Work_Environment:  225.00<br />Stress_Category: 1","Hiring_Potential:   8.56<br />Work_Environment:  361.44<br />Stress_Category: 1","Hiring_Potential:  24.22<br />Work_Environment:  505.56<br />Stress_Category: 1","Hiring_Potential:  17.55<br />Work_Environment:  381.06<br />Stress_Category: 1","Hiring_Potential:  -0.60<br />Work_Environment:  227.35<br />Stress_Category: 1","Hiring_Potential:  17.43<br />Work_Environment:  463.43<br />Stress_Category: 1","Hiring_Potential:   5.39<br />Work_Environment:  390.56<br />Stress_Category: 1","Hiring_Potential:  10.47<br />Work_Environment:  322.42<br />Stress_Category: 1","Hiring_Potential:   4.27<br />Work_Environment:  334.40<br />Stress_Category: 1","Hiring_Potential:   4.70<br />Work_Environment:  221.25<br />Stress_Category: 1","Hiring_Potential:  19.93<br />Work_Environment:  538.34<br />Stress_Category: 1","Hiring_Potential:  -7.24<br />Work_Environment:  265.08<br />Stress_Category: 1","Hiring_Potential:   6.48<br />Work_Environment:  385.20<br />Stress_Category: 1","Hiring_Potential:  10.00<br />Work_Environment:  371.70<br />Stress_Category: 1","Hiring_Potential:  17.56<br />Work_Environment:  361.44<br />Stress_Category: 1","Hiring_Potential:  21.58<br />Work_Environment:  587.28<br />Stress_Category: 1","Hiring_Potential:  24.10<br />Work_Environment:  318.29<br />Stress_Category: 1","Hiring_Potential:  14.70<br />Work_Environment:  788.64<br />Stress_Category: 1","Hiring_Potential:   6.65<br />Work_Environment:  230.85<br />Stress_Category: 1","Hiring_Potential:   4.38<br />Work_Environment:  204.40<br />Stress_Category: 1","Hiring_Potential:  12.76<br />Work_Environment:  404.10<br />Stress_Category: 1","Hiring_Potential:  12.14<br />Work_Environment:  449.10<br />Stress_Category: 1","Hiring_Potential:  28.04<br />Work_Environment:  716.21<br />Stress_Category: 1","Hiring_Potential:   9.71<br />Work_Environment:  556.40<br />Stress_Category: 1","Hiring_Potential:   5.81<br />Work_Environment:  359.28<br />Stress_Category: 1","Hiring_Potential:  -5.25<br />Work_Environment:  228.10<br />Stress_Category: 1","Hiring_Potential:   9.49<br />Work_Environment:  783.04<br />Stress_Category: 1","Hiring_Potential:  -3.59<br />Work_Environment:  222.95<br />Stress_Category: 1","Hiring_Potential:   8.43<br />Work_Environment:  371.70<br />Stress_Category: 1","Hiring_Potential:  32.98<br />Work_Environment:  978.80<br />Stress_Category: 1","Hiring_Potential: -16.96<br />Work_Environment:  160.64<br />Stress_Category: 1","Hiring_Potential:  15.69<br />Work_Environment:  581.14<br />Stress_Category: 1","Hiring_Potential:  -2.05<br />Work_Environment:  449.10<br />Stress_Category: 1","Hiring_Potential:   5.66<br />Work_Environment:  356.72<br />Stress_Category: 1","Hiring_Potential:  10.14<br />Work_Environment:  673.65<br />Stress_Category: 1","Hiring_Potential:   1.19<br />Work_Environment:  354.00<br />Stress_Category: 1","Hiring_Potential:   6.45<br />Work_Environment:  538.92<br />Stress_Category: 1","Hiring_Potential:  -0.17<br />Work_Environment:  642.56<br />Stress_Category: 1","Hiring_Potential:  12.85<br />Work_Environment:  884.73<br />Stress_Category: 1","Hiring_Potential:  -0.81<br />Work_Environment:  546.78<br />Stress_Category: 1","Hiring_Potential:  15.48<br />Work_Environment:  734.24<br />Stress_Category: 1","Hiring_Potential:  -3.66<br />Work_Environment:  657.16<br />Stress_Category: 1","Hiring_Potential:  -0.69<br />Work_Environment:  422.46<br />Stress_Category: 1","Hiring_Potential:  -9.28<br />Work_Environment:  351.76<br />Stress_Category: 1","Hiring_Potential:  -3.68<br />Work_Environment:  657.16<br />Stress_Category: 1","Hiring_Potential:  -9.33<br />Work_Environment:  595.36<br />Stress_Category: 1","Hiring_Potential:  -3.53<br />Work_Environment:  469.40<br />Stress_Category: 1","Hiring_Potential: -11.36<br />Work_Environment:  610.22<br />Stress_Category: 1","Hiring_Potential:  -7.86<br />Work_Environment:  454.50<br />Stress_Category: 1","Hiring_Potential:  -3.71<br />Work_Environment:  563.28<br />Stress_Category: 1","Hiring_Potential:  -6.53<br />Work_Environment:  504.36<br />Stress_Category: 1","Hiring_Potential:   0.50<br />Work_Environment:  494.01<br />Stress_Category: 1","Hiring_Potential: -12.35<br />Work_Environment:  440.90<br />Stress_Category: 1","Hiring_Potential: -10.48<br />Work_Environment:  610.22<br />Stress_Category: 1","Hiring_Potential: -11.43<br />Work_Environment:  611.91<br />Stress_Category: 1","Hiring_Potential:  -8.37<br />Work_Environment:  719.82<br />Stress_Category: 1","Hiring_Potential: -17.68<br />Work_Environment:  520.32<br />Stress_Category: 1","Hiring_Potential: -12.78<br />Work_Environment:  740.55<br />Stress_Category: 1","Hiring_Potential:   3.35<br />Work_Environment:  812.06<br />Stress_Category: 1","Hiring_Potential:  -6.78<br />Work_Environment:  753.10<br />Stress_Category: 1","Hiring_Potential: -12.62<br />Work_Environment:  756.80<br />Stress_Category: 1","Hiring_Potential:  -5.00<br />Work_Environment:  687.42<br />Stress_Category: 1","Hiring_Potential:  -2.93<br />Work_Environment:  796.74<br />Stress_Category: 1","Hiring_Potential:  -6.75<br />Work_Environment:  489.60<br />Stress_Category: 1","Hiring_Potential: -31.88<br />Work_Environment:  597.24<br />Stress_Category: 1","Hiring_Potential: -11.22<br />Work_Environment:  669.90<br />Stress_Category: 1","Hiring_Potential: -40.76<br />Work_Environment:  728.32<br />Stress_Category: 1","Hiring_Potential:   0.79<br />Work_Environment:  973.94<br />Stress_Category: 1","Hiring_Potential: -13.62<br />Work_Environment:  796.86<br />Stress_Category: 1","Hiring_Potential:  -2.47<br />Work_Environment:  868.32<br />Stress_Category: 1","Hiring_Potential:  -5.07<br />Work_Environment:  682.02<br />Stress_Category: 1","Hiring_Potential:  -5.50<br />Work_Environment:  987.80<br />Stress_Category: 1"],"type":"scatter","mode":"markers","marker":{"autocolorscale":false,"color":"rgba(183,159,0,1)","opacity":1,"size":5.66929133858268,"symbol":"circle","line":{"width":1.88976377952756,"color":"rgba(183,159,0,1)"}},"hoveron":"points","name":"1","legendgroup":"1","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[8.3,14.33,22.39,6.22,7.19,8.86,6.52,1,17.65,19.11,-1.61,15.11,14.74,25.75,1.36,25.9,14.26,7.54,14.44,-2.65,6.79,-3.54,8.79,20.65,-2.58,6.35,9.42,17.54,1.07,8.32,4.18,-2.68,11.03,18.9,13.97,3.82,2.65,10.89,10.38,-1.24,6.86,1.18,5.19,5.22,0.53,3.39,0.67,-1.25,2.28,0.69,-3.58,-0.16,-11.25,8.74,-6.4,-2,8.14,0,-0.92,4.5,-14.14,-30.29,-15.74,-19.57],"y":[230.3,314.37,322.42,230.3,432.99,275,274.68,177,940.59,428,655.98,737.85,449.1,500.17,278.46,545.64,626.52,550.42,1119.75,318.92,727.52,486.75,752.25,1471.5,493.79,486.75,1417,633.5,807.12,709.6,665.21,610.22,1015.4,1080,956.97,836.38,663.75,872.96,889.14,855.76,796.86,981.2,945.8,1263.84,836.43,1055.47,1124.48,1151.02,968.2,1136.25,1461.24,973.36,1062,1368.32,929.67,1100.55,1474.48,947.25,1266.14,1660.56,1084.59,1127.36,1180.14,1731.45],"text":["Hiring_Potential:   8.30<br />Work_Environment:  230.30<br />Stress_Category: 2","Hiring_Potential:  14.33<br />Work_Environment:  314.37<br />Stress_Category: 2","Hiring_Potential:  22.39<br />Work_Environment:  322.42<br />Stress_Category: 2","Hiring_Potential:   6.22<br />Work_Environment:  230.30<br />Stress_Category: 2","Hiring_Potential:   7.19<br />Work_Environment:  432.99<br />Stress_Category: 2","Hiring_Potential:   8.86<br />Work_Environment:  275.00<br />Stress_Category: 2","Hiring_Potential:   6.52<br />Work_Environment:  274.68<br />Stress_Category: 2","Hiring_Potential:   1.00<br />Work_Environment:  177.00<br />Stress_Category: 2","Hiring_Potential:  17.65<br />Work_Environment:  940.59<br />Stress_Category: 2","Hiring_Potential:  19.11<br />Work_Environment:  428.00<br />Stress_Category: 2","Hiring_Potential:  -1.61<br />Work_Environment:  655.98<br />Stress_Category: 2","Hiring_Potential:  15.11<br />Work_Environment:  737.85<br />Stress_Category: 2","Hiring_Potential:  14.74<br />Work_Environment:  449.10<br />Stress_Category: 2","Hiring_Potential:  25.75<br />Work_Environment:  500.17<br />Stress_Category: 2","Hiring_Potential:   1.36<br />Work_Environment:  278.46<br />Stress_Category: 2","Hiring_Potential:  25.90<br />Work_Environment:  545.64<br />Stress_Category: 2","Hiring_Potential:  14.26<br />Work_Environment:  626.52<br />Stress_Category: 2","Hiring_Potential:   7.54<br />Work_Environment:  550.42<br />Stress_Category: 2","Hiring_Potential:  14.44<br />Work_Environment: 1119.75<br />Stress_Category: 2","Hiring_Potential:  -2.65<br />Work_Environment:  318.92<br />Stress_Category: 2","Hiring_Potential:   6.79<br />Work_Environment:  727.52<br />Stress_Category: 2","Hiring_Potential:  -3.54<br />Work_Environment:  486.75<br />Stress_Category: 2","Hiring_Potential:   8.79<br />Work_Environment:  752.25<br />Stress_Category: 2","Hiring_Potential:  20.65<br />Work_Environment: 1471.50<br />Stress_Category: 2","Hiring_Potential:  -2.58<br />Work_Environment:  493.79<br />Stress_Category: 2","Hiring_Potential:   6.35<br />Work_Environment:  486.75<br />Stress_Category: 2","Hiring_Potential:   9.42<br />Work_Environment: 1417.00<br />Stress_Category: 2","Hiring_Potential:  17.54<br />Work_Environment:  633.50<br />Stress_Category: 2","Hiring_Potential:   1.07<br />Work_Environment:  807.12<br />Stress_Category: 2","Hiring_Potential:   8.32<br />Work_Environment:  709.60<br />Stress_Category: 2","Hiring_Potential:   4.18<br />Work_Environment:  665.21<br />Stress_Category: 2","Hiring_Potential:  -2.68<br />Work_Environment:  610.22<br />Stress_Category: 2","Hiring_Potential:  11.03<br />Work_Environment: 1015.40<br />Stress_Category: 2","Hiring_Potential:  18.90<br />Work_Environment: 1080.00<br />Stress_Category: 2","Hiring_Potential:  13.97<br />Work_Environment:  956.97<br />Stress_Category: 2","Hiring_Potential:   3.82<br />Work_Environment:  836.38<br />Stress_Category: 2","Hiring_Potential:   2.65<br />Work_Environment:  663.75<br />Stress_Category: 2","Hiring_Potential:  10.89<br />Work_Environment:  872.96<br />Stress_Category: 2","Hiring_Potential:  10.38<br />Work_Environment:  889.14<br />Stress_Category: 2","Hiring_Potential:  -1.24<br />Work_Environment:  855.76<br />Stress_Category: 2","Hiring_Potential:   6.86<br />Work_Environment:  796.86<br />Stress_Category: 2","Hiring_Potential:   1.18<br />Work_Environment:  981.20<br />Stress_Category: 2","Hiring_Potential:   5.19<br />Work_Environment:  945.80<br />Stress_Category: 2","Hiring_Potential:   5.22<br />Work_Environment: 1263.84<br />Stress_Category: 2","Hiring_Potential:   0.53<br />Work_Environment:  836.43<br />Stress_Category: 2","Hiring_Potential:   3.39<br />Work_Environment: 1055.47<br />Stress_Category: 2","Hiring_Potential:   0.67<br />Work_Environment: 1124.48<br />Stress_Category: 2","Hiring_Potential:  -1.25<br />Work_Environment: 1151.02<br />Stress_Category: 2","Hiring_Potential:   2.28<br />Work_Environment:  968.20<br />Stress_Category: 2","Hiring_Potential:   0.69<br />Work_Environment: 1136.25<br />Stress_Category: 2","Hiring_Potential:  -3.58<br />Work_Environment: 1461.24<br />Stress_Category: 2","Hiring_Potential:  -0.16<br />Work_Environment:  973.36<br />Stress_Category: 2","Hiring_Potential: -11.25<br />Work_Environment: 1062.00<br />Stress_Category: 2","Hiring_Potential:   8.74<br />Work_Environment: 1368.32<br />Stress_Category: 2","Hiring_Potential:  -6.40<br />Work_Environment:  929.67<br />Stress_Category: 2","Hiring_Potential:  -2.00<br />Work_Environment: 1100.55<br />Stress_Category: 2","Hiring_Potential:   8.14<br />Work_Environment: 1474.48<br />Stress_Category: 2","Hiring_Potential:   0.00<br />Work_Environment:  947.25<br />Stress_Category: 2","Hiring_Potential:  -0.92<br />Work_Environment: 1266.14<br />Stress_Category: 2","Hiring_Potential:   4.50<br />Work_Environment: 1660.56<br />Stress_Category: 2","Hiring_Potential: -14.14<br />Work_Environment: 1084.59<br />Stress_Category: 2","Hiring_Potential: -30.29<br />Work_Environment: 1127.36<br />Stress_Category: 2","Hiring_Potential: -15.74<br />Work_Environment: 1180.14<br />Stress_Category: 2","Hiring_Potential: -19.57<br />Work_Environment: 1731.45<br />Stress_Category: 2"],"type":"scatter","mode":"markers","marker":{"autocolorscale":false,"color":"rgba(0,186,56,1)","opacity":1,"size":5.66929133858268,"symbol":"circle","line":{"width":1.88976377952756,"color":"rgba(0,186,56,1)"}},"hoveron":"points","name":"2","legendgroup":"2","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[5.09,21.29,10.11,37.05,20.14,20.58,9.42,5.93,4.7,10.1,16.57,5.45,-7.09,13.63,1.08,-3.93,9.5,-2.91,-4.17,7.11,-3.48,4.68,-9.32,-12.73],"y":[322.42,863.93,1261.5,1295.8,1018.75,1962,1085.52,1195,1155.75,923.21,1293.57,830.28,929.25,821.88,1293.84,1108.08,1646.75,1293.57,1106.25,1555.85,1034.02,1610.7,1481.2,1593.72],"text":["Hiring_Potential:   5.09<br />Work_Environment:  322.42<br />Stress_Category: 3","Hiring_Potential:  21.29<br />Work_Environment:  863.93<br />Stress_Category: 3","Hiring_Potential:  10.11<br />Work_Environment: 1261.50<br />Stress_Category: 3","Hiring_Potential:  37.05<br />Work_Environment: 1295.80<br />Stress_Category: 3","Hiring_Potential:  20.14<br />Work_Environment: 1018.75<br />Stress_Category: 3","Hiring_Potential:  20.58<br />Work_Environment: 1962.00<br />Stress_Category: 3","Hiring_Potential:   9.42<br />Work_Environment: 1085.52<br />Stress_Category: 3","Hiring_Potential:   5.93<br />Work_Environment: 1195.00<br />Stress_Category: 3","Hiring_Potential:   4.70<br />Work_Environment: 1155.75<br />Stress_Category: 3","Hiring_Potential:  10.10<br />Work_Environment:  923.21<br />Stress_Category: 3","Hiring_Potential:  16.57<br />Work_Environment: 1293.57<br />Stress_Category: 3","Hiring_Potential:   5.45<br />Work_Environment:  830.28<br />Stress_Category: 3","Hiring_Potential:  -7.09<br />Work_Environment:  929.25<br />Stress_Category: 3","Hiring_Potential:  13.63<br />Work_Environment:  821.88<br />Stress_Category: 3","Hiring_Potential:   1.08<br />Work_Environment: 1293.84<br />Stress_Category: 3","Hiring_Potential:  -3.93<br />Work_Environment: 1108.08<br />Stress_Category: 3","Hiring_Potential:   9.50<br />Work_Environment: 1646.75<br />Stress_Category: 3","Hiring_Potential:  -2.91<br />Work_Environment: 1293.57<br />Stress_Category: 3","Hiring_Potential:  -4.17<br />Work_Environment: 1106.25<br />Stress_Category: 3","Hiring_Potential:   7.11<br />Work_Environment: 1555.85<br />Stress_Category: 3","Hiring_Potential:  -3.48<br />Work_Environment: 1034.02<br />Stress_Category: 3","Hiring_Potential:   4.68<br />Work_Environment: 1610.70<br />Stress_Category: 3","Hiring_Potential:  -9.32<br />Work_Environment: 1481.20<br />Stress_Category: 3","Hiring_Potential: -12.73<br />Work_Environment: 1593.72<br />Stress_Category: 3"],"type":"scatter","mode":"markers","marker":{"autocolorscale":false,"color":"rgba(0,191,196,1)","opacity":1,"size":5.66929133858268,"symbol":"circle","line":{"width":1.88976377952756,"color":"rgba(0,191,196,1)"}},"hoveron":"points","name":"3","legendgroup":"3","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[-0.95,2.6,-2.59,-0.44,2.84,-0.84,7.71,-8.53,7.85,2.09,-14.25,5.27,0.09],"y":[1010.1,1247.13,1540,929.25,803,1371.75,1733.4,1134.77,1877.85,1327.5,1106.25,2317.21,1817.53],"text":["Hiring_Potential:  -0.95<br />Work_Environment: 1010.10<br />Stress_Category: 4","Hiring_Potential:   2.60<br />Work_Environment: 1247.13<br />Stress_Category: 4","Hiring_Potential:  -2.59<br />Work_Environment: 1540.00<br />Stress_Category: 4","Hiring_Potential:  -0.44<br />Work_Environment:  929.25<br />Stress_Category: 4","Hiring_Potential:   2.84<br />Work_Environment:  803.00<br />Stress_Category: 4","Hiring_Potential:  -0.84<br />Work_Environment: 1371.75<br />Stress_Category: 4","Hiring_Potential:   7.71<br />Work_Environment: 1733.40<br />Stress_Category: 4","Hiring_Potential:  -8.53<br />Work_Environment: 1134.77<br />Stress_Category: 4","Hiring_Potential:   7.85<br />Work_Environment: 1877.85<br />Stress_Category: 4","Hiring_Potential:   2.09<br />Work_Environment: 1327.50<br />Stress_Category: 4","Hiring_Potential: -14.25<br />Work_Environment: 1106.25<br />Stress_Category: 4","Hiring_Potential:   5.27<br />Work_Environment: 2317.21<br />Stress_Category: 4","Hiring_Potential:   0.09<br />Work_Environment: 1817.53<br />Stress_Category: 4"],"type":"scatter","mode":"markers","marker":{"autocolorscale":false,"color":"rgba(97,156,255,1)","opacity":1,"size":5.66929133858268,"symbol":"circle","line":{"width":1.88976377952756,"color":"rgba(97,156,255,1)"}},"hoveron":"points","name":"4","legendgroup":"4","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[5.53,18.22],"y":[1064.16,3314.03],"text":["Hiring_Potential:   5.53<br />Work_Environment: 1064.16<br />Stress_Category: 5","Hiring_Potential:  18.22<br />Work_Environment: 3314.03<br />Stress_Category: 5"],"type":"scatter","mode":"markers","marker":{"autocolorscale":false,"color":"rgba(245,100,227,1)","opacity":1,"size":5.66929133858268,"symbol":"circle","line":{"width":1.88976377952756,"color":"rgba(245,100,227,1)"}},"hoveron":"points","name":"5","legendgroup":"5","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":26.2283105022831,"r":7.30593607305936,"b":40.1826484018265,"l":48.9497716894977},"plot_bgcolor":"rgba(235,235,235,1)","paper_bgcolor":"rgba(255,255,255,1)","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-44.6505,40.9405],"tickmode":"array","ticktext":["-40","-20","0","20","40"],"tickvals":[-40,-20,0,20,40],"categoryorder":"array","categoryarray":["-40","-20","0","20","40"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.65296803652968,"tickwidth":0.66417600664176,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(255,255,255,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"Hiring_Potential","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-71.7055,3475.2555],"tickmode":"array","ticktext":["0","1000","2000","3000"],"tickvals":[0,1000,2000,3000],"categoryorder":"array","categoryarray":["0","1000","2000","3000"],"nticks":null,"ticks":"outside","tickcolor":"rgba(51,51,51,1)","ticklen":3.65296803652968,"tickwidth":0.66417600664176,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(255,255,255,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"Work_Environment","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":true,"legend":{"bgcolor":"rgba(255,255,255,1)","bordercolor":"transparent","borderwidth":1.88976377952756,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895},"y":0.913385826771654},"annotations":[{"text":"Stress_Category","x":1.02,"y":1,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"left","yanchor":"bottom","legendTitle":true}],"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"1812b43c19af6":{"x":{},"y":{},"colour":{},"type":"scatter"}},"cur_data":"1812b43c19af6","visdat":{"1812b43c19af6":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->


# Split the data 90:10 training:testing 

```r
set.seed(12345)
subset_int <- sample(nrow(job_data),floor(nrow(job_data)*0.9))  # 90% training + 10% testing
```


```r
job_data_train<-job_data[subset_int, ]
job_data_test<-job_data[-subset_int, ]

job_data_dtm_train<-job_data_dtm[subset_int, ]
job_data_dtm_test<-job_data_dtm[-subset_int, ]

corpus_train <- corpus_clean[subset_int]
corpus_test <- corpus_clean[-subset_int]
```



```r
round(prop.table(table(job_data_train$Stress_Category)), digits = 2)
```

```
## 
##    0    1    2    3    4    5 
## 0.07 0.43 0.31 0.12 0.07 0.01
```

```r
round(prop.table(table(job_data_test$Stress_Category)), digits = 2)
```

```
## 
##    0    1    2    3    4    5 
## 0.00 0.40 0.45 0.15 0.00 0.00
```




# Binarize Job Stress
## Binarize the Job Stress for training
- Binarize the Job Stress into two categories (low/high stress levels)
- `a %in% b` is an intuitive interface to match and acts as a binary operator returning a logical vector (T or F) indicating if there is a match between the left and right operands.

```r
plot(job_data_train$Stress_Category)
```

<img src="/project/textAnalysisJobSatisfaction_DSPA4/index_files/figure-html/unnamed-chunk-23-1.png" width="672" />

- Binarize the Job Stress into two categories (low/high stress levels)

```r
job_data_train$stress_binary <- job_data_train$Stress_Category %in% c(3:5)
job_data_train$stress_binary <- factor(job_data_train$stress_binary, levels=c(F, T), labels = c("low_stress", "high_stress"))
```

## Binarize the Job Stress for testing

```r
plot(job_data_test$Stress_Category)
```

<img src="/project/textAnalysisJobSatisfaction_DSPA4/index_files/figure-html/unnamed-chunk-25-1.png" width="672" />

- Binarize the Job Stress into two categories (low/high stress levels)

```r
job_data_test$stress_binary <- job_data_test$Stress_Category %in% c(3:5)
job_data_test$stress_binary <- factor(job_data_test$stress_binary, levels=c(F, T), labels = c("low_stress", "high_stress"))
```

## Compare the proportions

```r
prop.table(table(job_data_train$stress_binary))
```

```
## 
##  low_stress high_stress 
##         0.8         0.2
```

```r
prop.table(table(job_data_test$stress_binary))
```

```
## 
##  low_stress high_stress 
##        0.85        0.15
```


# Word Cloud & Visualization

## Word Cloud to Visualize Training Data Job Desc

```r
# install.packages("wordcloud", repos = "http://cran.us.r-project.org")
library(wordcloud)
```

```
## Warning: package 'wordcloud' was built under R version 3.4.4
```

```
## Loading required package: RColorBrewer
```

```r
library(RColorBrewer)
library(wesanderson)
```

```
## Warning: package 'wesanderson' was built under R version 3.4.4
```


```r
pal <- rainbow(10)
library(wordcloud)
wordcloud(corpus_train, max.words =100, min.freq=10, scale=c(4,.5), 
           random.order = FALSE, rot.per=.5, vfont=c("sans serif","plain"), colors=pal)
```

<img src="/project/textAnalysisJobSatisfaction_DSPA4/index_files/figure-html/unnamed-chunk-29-1.png" width="672" />

## Visualize differences between low and high stress categories

```r
low_stress<-subset(job_data_train, stress_binary=="low_stress")
wordcloud(low_stress$Description, max.words = 20, min.freq=5, scale=c(4,.5), 
           random.order = FALSE, rot.per=.2, vfont=c("sans serif","plain"), colors=pal)
```

<img src="/project/textAnalysisJobSatisfaction_DSPA4/index_files/figure-html/unnamed-chunk-30-1.png" width="672" />


```r
high_stress<-subset(job_data_train, stress_binary=="high_stress")
wordcloud(high_stress$Description, max.words = 20, min.freq=2, scale=c(4,.5), 
           random.order = FALSE, rot.per=.2, vfont=c("sans serif","plain"), colors=pal)
```

<img src="/project/textAnalysisJobSatisfaction_DSPA4/index_files/figure-html/unnamed-chunk-31-1.png" width="672" />


```r
# Words in job description by frequency
freq_dtm_train <- sort(colSums(as.matrix(job_data_dtm_train)), decreasing=TRUE)
freq_dtm_test <- sort(colSums(as.matrix(job_data_dtm_test)), decreasing=TRUE)
```


```r
ggplot(job_data_train, aes(x=Hiring_Potential)) +
    geom_density(binwidth=.5, fill="white") + ggtitle("Hiring Potential Frequency")+ facet_wrap(~stress_binary) 
```

```
## Warning: Ignoring unknown parameters: binwidth
```

<img src="/project/textAnalysisJobSatisfaction_DSPA4/index_files/figure-html/unnamed-chunk-33-1.png" width="672" />

# Word count into categorical data
- we are going to make frequencies of words into features.
- we will create indicators for words that appear at least in 3 different documents in the training data.

```r
summary(findFreqTerms(job_data_dtm_train, 2)) 
```

```
##    Length     Class      Mode 
##       303 character character
```



```r
job_data_dict <- as.character(findFreqTerms(job_data_dtm_train, 3))
job_train <- DocumentTermMatrix(corpus_train, list(dictionary=job_data_dict))
job_test <- DocumentTermMatrix(corpus_test, list(dictionary=job_data_dict))
```

- The Naive Bayes classifier trains on data with categorical features, as it uses frequency tables for learning the data affinities. To create the combinations of class and feature values comprising the frequency-table (matrix), all feature must be categorical.

```r
convert_counts <- function(wordFreq) {
  wordFreq <- ifelse(wordFreq > 0, 1, 0)
  wordFreq <- factor(wordFreq, levels = c(0, 1), labels = c("No", "Yes"))
  return(wordFreq)
}
```


```r
job_train <- apply(job_train, MARGIN = 2, convert_counts)
job_test <- apply(job_test, MARGIN = 2, convert_counts)

# Check the structure of the data 
dim(job_train)
```

```
## [1] 180 142
```

```r
dim(job_test)
```

```
## [1]  20 142
```

# Report sparsity 
- High sparsity of about 97%, indicating mostly "No" values. 

```r
prop.table(table(job_train))
```

```
## job_train
##         No        Yes 
## 0.97296557 0.02703443
```

```r
prop.table(table(job_test))
```

```
## job_test
##         No        Yes 
## 0.97535211 0.02464789
```

# Analyctics 
## Naive Bayes
- Apply the Naive Bayes classifier on the high frequency terms to predict stress level (low/high).

```r
# install.packages("e1071", repos = "http://cran.us.r-project.org")
library(e1071)
```

### Build classifier 

```r
job_classifier <- naiveBayes(job_train, job_data_train$stress_binary)
```


```r
job_test_pred<-predict(job_classifier, job_test)
```

### Evaluate model performance
- Accuracy is 80%. 

```r
library(gmodels)
```



```r
table_NB1 <- CrossTable(job_test_pred, job_data_test$stress_binary)
```

```
## 
##  
##    Cell Contents
## |-------------------------|
## |                       N |
## | Chi-square contribution |
## |           N / Row Total |
## |           N / Col Total |
## |         N / Table Total |
## |-------------------------|
## 
##  
## Total Observations in Table:  20 
## 
##  
##               | job_data_test$stress_binary 
## job_test_pred |  low_stress | high_stress |   Row Total | 
## --------------|-------------|-------------|-------------|
##    low_stress |          16 |           3 |          19 | 
##               |       0.001 |       0.008 |             | 
##               |       0.842 |       0.158 |       0.950 | 
##               |       0.941 |       1.000 |             | 
##               |       0.800 |       0.150 |             | 
## --------------|-------------|-------------|-------------|
##   high_stress |           1 |           0 |           1 | 
##               |       0.026 |       0.150 |             | 
##               |       1.000 |       0.000 |       0.050 | 
##               |       0.059 |       0.000 |             | 
##               |       0.050 |       0.000 |             | 
## --------------|-------------|-------------|-------------|
##  Column Total |          17 |           3 |          20 | 
##               |       0.850 |       0.150 |             | 
## --------------|-------------|-------------|-------------|
## 
## 
```

- Accuracy of Naive Bayes 

```r
acc_NB1 <- (table_NB1$t[1,1]+table_NB1$t[2,2])/dim(job_data_test)[1]
print(acc_NB1)
```

```
## [1] 0.8
```

## LDA Prediction 

```r
library(MASS)
```


```r
df_job_train <- data.frame(lapply(as.data.frame(job_train),as.numeric), stage = job_data_train$stress_binary)
df_job_test <- data.frame(lapply(as.data.frame(job_test),as.numeric), stage = job_data_test$stress_binary)

set.seed(1234)
job_lda <- lda(data=df_job_train, stage~.)
# hn_pred = predict(hn_lda, df_hn_test[,-104])
hn_pred = predict(job_lda, df_job_test)
```


```r
table_LDA1 <- CrossTable(hn_pred$class, df_job_test$stage)
```

```
## 
##  
##    Cell Contents
## |-------------------------|
## |                       N |
## | Chi-square contribution |
## |           N / Row Total |
## |           N / Col Total |
## |         N / Table Total |
## |-------------------------|
## 
##  
## Total Observations in Table:  20 
## 
##  
##               | df_job_test$stage 
## hn_pred$class |  low_stress | high_stress |   Row Total | 
## --------------|-------------|-------------|-------------|
##    low_stress |          14 |           2 |          16 | 
##               |       0.012 |       0.067 |             | 
##               |       0.875 |       0.125 |       0.800 | 
##               |       0.824 |       0.667 |             | 
##               |       0.700 |       0.100 |             | 
## --------------|-------------|-------------|-------------|
##   high_stress |           3 |           1 |           4 | 
##               |       0.047 |       0.267 |             | 
##               |       0.750 |       0.250 |       0.200 | 
##               |       0.176 |       0.333 |             | 
##               |       0.150 |       0.050 |             | 
## --------------|-------------|-------------|-------------|
##  Column Total |          17 |           3 |          20 | 
##               |       0.850 |       0.150 |             | 
## --------------|-------------|-------------|-------------|
## 
## 
```

- Accuracy of LDA

```r
# accuracy of LDA
acc_LDA1 <- (table_LDA1$t[1,1]+table_LDA1$t[2,2])/dim(job_data_test)[1]
print(acc_LDA1)
```

```
## [1] 0.75
```

- Error of LDA

```r
# error of LDA
error_LDA1 <- (table_LDA1$t[1,2]+table_LDA1$t[2,1])/dim(job_data_test)[1]
print(error_LDA1)
```

```
## [1] 0.25
```


- Specificity: TN/(TN+FP)

```r
# specifity of LDA
specificity_LDA1 <- (table_LDA1$t[1,1]/(table_LDA1$t[1,1] + table_LDA1$t[2,1]))
specificity_LDA1
```

```
## [1] 0.8235294
```

- Sensitivity: TP/(TP+FN)

```r
sensitivity_LDA1 <- (table_LDA1$t[2,1]/(table_LDA1$t[2,1] + table_LDA1$t[2,2]))
sensitivity_LDA1
```

```
## [1] 0.75
```


## Decision Tree

```r
# install.packages("C50")
library(C50)
```


```r
summary(job_data_train[,-c(1,5,6)])
```

```
##  Overall_Score   Average_Income(USD) Work_Environment  Physical_Demand 
##  Min.   : 60.0   Min.   : 18053      Min.   :  89.72   Min.   : 3.970  
##  1st Qu.:367.0   1st Qu.: 33200      1st Qu.: 426.62   1st Qu.: 7.237  
##  Median :528.5   Median : 46276      Median : 671.77   Median : 9.970  
##  Mean   :505.8   Mean   : 54048      Mean   : 767.03   Mean   :12.868  
##  3rd Qu.:645.0   3rd Qu.: 62458      3rd Qu.:1011.42   3rd Qu.:15.863  
##  Max.   :892.0   Max.   :365258      Max.   :3314.03   Max.   :43.230  
##  Hiring_Potential  Description            stress_binary
##  Min.   :-40.760   Length:180         low_stress :144  
##  1st Qu.: -3.583   Class :character   high_stress: 36  
##  Median :  4.325   Mode  :character                    
##  Mean   :  3.581                                       
##  3rd Qu.: 10.575                                       
##  Max.   : 37.050
```

```r
colnames(job_data_train)
```

```
##  [1] "Job_Title"           "Overall_Score"       "Average_Income(USD)"
##  [4] "Work_Environment"    "Stress_Level"        "Stress_Category"    
##  [7] "Physical_Demand"     "Hiring_Potential"    "Description"        
## [10] "stress_binary"
```

```r
set.seed(1234)
job_model <-C5.0(job_data_train[,-c(1,5,6,9)], job_data_train$stress_binary)
job_model
```

```
## 
## Call:
## C5.0.default(x = job_data_train[, -c(1, 5, 6, 9)], y
##  = job_data_train$stress_binary)
## 
## Classification Tree
## Number of samples: 180 
## Number of predictors: 6 
## 
## Tree size: 2 
## 
## Non-standard options: attempt to group attributes
```

```r
summary(job_model)
```

```
## 
## Call:
## C5.0.default(x = job_data_train[, -c(1, 5, 6, 9)], y
##  = job_data_train$stress_binary)
## 
## 
## C5.0 [Release 2.07 GPL Edition]  	Wed Jun 19 19:03:24 2019
## -------------------------------
## 
## Class specified by attribute `outcome'
## 
## Read 180 cases (7 attributes) from undefined.data
## 
## Decision tree:
## 
## stress_binary = low_stress: low_stress (144)
## stress_binary = high_stress: high_stress (36)
## 
## 
## Evaluation on training data (180 cases):
## 
## 	    Decision Tree   
## 	  ----------------  
## 	  Size      Errors  
## 
## 	     2    0( 0.0%)   <<
## 
## 
## 	   (a)   (b)    <-classified as
## 	  ----  ----
## 	   144          (a): class low_stress
## 	          36    (b): class high_stress
## 
## 
## 	Attribute usage:
## 
## 	100.00%	stress_binary
## 
## 
## Time: 0.0 secs
```

- Accuracy is 1

```r
job_pred<-predict(job_model, job_data_test[,-c(1,5,6,9)])
library(caret)
```

```
## Warning: package 'caret' was built under R version 3.4.4
```

```
## Loading required package: lattice
```

```
## Warning: package 'lattice' was built under R version 3.4.4
```

```r
confusionMatrix(table(job_pred, job_data_test$stress_binary))
```

```
## Confusion Matrix and Statistics
## 
##              
## job_pred      low_stress high_stress
##   low_stress          17           0
##   high_stress          0           3
##                                      
##                Accuracy : 1          
##                  95% CI : (0.8316, 1)
##     No Information Rate : 0.85       
##     P-Value [Acc > NIR] : 0.03876    
##                                      
##                   Kappa : 1          
##  Mcnemar's Test P-Value : NA         
##                                      
##             Sensitivity : 1.00       
##             Specificity : 1.00       
##          Pos Pred Value : 1.00       
##          Neg Pred Value : 1.00       
##              Prevalence : 0.85       
##          Detection Rate : 0.85       
##    Detection Prevalence : 0.85       
##       Balanced Accuracy : 1.00       
##                                      
##        'Positive' Class : low_stress 
## 
```

## Multivariate Linear Model 
- To predict Overall job ranking (smaller is better). Generate some informative pairs plots. Use backward step-wise feature selection to simplify the model, report the AIC.

```r
colnames(job_data)
```

```
## [1] "Job_Title"           "Overall_Score"       "Average_Income(USD)"
## [4] "Work_Environment"    "Stress_Level"        "Stress_Category"    
## [7] "Physical_Demand"     "Hiring_Potential"    "Description"
```

```r
fit <- lm(Overall_Score ~., data=job_data[,-c(1,9)])
summary(fit)
```

```
## 
## Call:
## lm(formula = Overall_Score ~ ., data = job_data[, -c(1, 9)])
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -217.915  -37.741   -2.335   34.054  221.189 
## 
## Coefficients:
##                         Estimate Std. Error t value Pr(>|t|)    
## (Intercept)            2.753e+02  2.122e+01  12.971  < 2e-16 ***
## `Average_Income(USD)` -1.200e-03  1.259e-04  -9.531  < 2e-16 ***
## Work_Environment       1.805e-01  1.718e-02  10.507  < 2e-16 ***
## Stress_Level           4.931e+00  1.461e+00   3.375 0.000897 ***
## Stress_Category1      -9.948e+00  2.034e+01  -0.489 0.625305    
## Stress_Category2       9.258e+00  2.975e+01   0.311 0.755974    
## Stress_Category3      -7.648e+00  4.278e+01  -0.179 0.858322    
## Stress_Category4      -7.113e+01  5.587e+01  -1.273 0.204560    
## Stress_Category5      -3.173e+02  8.602e+01  -3.688 0.000295 ***
## Physical_Demand        5.945e+00  7.765e-01   7.656  9.6e-13 ***
## Hiring_Potential      -5.925e+00  3.608e-01 -16.422  < 2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 55.02 on 189 degrees of freedom
## Multiple R-squared:  0.9173,	Adjusted R-squared:  0.9129 
## F-statistic: 209.7 on 10 and 189 DF,  p-value: < 2.2e-16
```



```r
plot(fit, which = 1:2)
```

<img src="/project/textAnalysisJobSatisfaction_DSPA4/index_files/figure-html/unnamed-chunk-58-1.png" width="672" /><img src="/project/textAnalysisJobSatisfaction_DSPA4/index_files/figure-html/unnamed-chunk-58-2.png" width="672" />

- AIC=1613.73

```r
step(fit,direction = "backward")
```

```
## Start:  AIC=1613.73
## Overall_Score ~ `Average_Income(USD)` + Work_Environment + Stress_Level + 
##     Stress_Category + Physical_Demand + Hiring_Potential
## 
##                         Df Sum of Sq     RSS    AIC
## <none>                                572051 1613.7
## - Stress_Level           1     34470  606521 1623.4
## - Stress_Category        5    187299  759350 1660.4
## - Physical_Demand        1    177420  749472 1665.8
## - `Average_Income(USD)`  1    274953  847004 1690.2
## - Work_Environment       1    334145  906196 1703.7
## - Hiring_Potential       1    816211 1388262 1789.0
```

```
## 
## Call:
## lm(formula = Overall_Score ~ `Average_Income(USD)` + Work_Environment + 
##     Stress_Level + Stress_Category + Physical_Demand + Hiring_Potential, 
##     data = job_data[, -c(1, 9)])
## 
## Coefficients:
##           (Intercept)  `Average_Income(USD)`       Work_Environment  
##              275.2575                -0.0012                 0.1805  
##          Stress_Level       Stress_Category1       Stress_Category2  
##                4.9308                -9.9480                 9.2579  
##      Stress_Category3       Stress_Category4       Stress_Category5  
##               -7.6480               -71.1279              -317.2594  
##       Physical_Demand       Hiring_Potential  
##                5.9454                -5.9251
```
