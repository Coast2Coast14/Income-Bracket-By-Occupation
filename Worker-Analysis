* Reading in Data ;
data census;
infile "/home/u55770877/sasuser.v94/adult-test.csv" firstobs=2 delimiter=",";
input Age Workclass $ fnlwgt Education $ Education_Num Marital_Status $ Occupation $ Relationship $ Race $ Gender $ Capital_Gains Capital_Losses Hours_Per_Week Native_Country $ Income_Bracket $;
if income_bracket = "<=50K." then num_income = 0;
else num_income = 1;
run;

* Macros ;
%MACRO graphAvg (dataset=, x=, y=);
proc sgplot data = &dataset;
where _stat_ = 'MEAN';
vbar &x / freq = &y;
title "Average &y By &x";
yaxis label = "Average &y";
run;
%Mend;

%MACRO graphSum (dataset=, x=, y=);
proc sgplot data = &dataset;
title "Total number of people with income >=50k by &x";
vbar &x / freq=&y stat=sum;
run;
%Mend;

* Calculating averages to store in table ;
* By Occupation ;
proc means data = census;
class occupation;
var hours_per_week capital_gains capital_losses education_num age;
output out = work.census_occupation;
run;

* By education ;
proc means data = census;
class education;
var hours_per_week capital_gains capital_losses;
output out = work.census_education;
run;

* Graphs by occupation ;
proc sgplot data = census;
title "Frequency of Each Occupation";
vbar occupation;
run;

* Avg Hours per week by occupation ;
%graphAvg(dataset=work.census_occupation, x=occupation, y=hours_per_week);

* Avg capital_gains by occupation ;
%graphAvg(dataset=work.census_occupation, x=occupation, y=capital_gains);

* Avg capital_losses by occupation ;
%graphAvg(dataset=work.census_occupation, x=occupation, y=capital_losses);

* Avg age by occupation ;
%graphAvg(dataset=work.census_occupation, x=occupation, y=age);

proc gchart data = census;
pie education /other = 2;
run;

* Avg Hours per week by education ;
%graphAvg(dataset=work.census_education, x=education, y=hours_per_week);

* Avg capital_gains by education ;
%graphAvg(dataset=work.census_education, x=education, y=capital_gains);

* Avg capital_losses by education ;
%graphAvg(dataset=work.census_education, x=education, y=capital_losses);

* Sum Graphs ;

proc sort data = census;
by Income_Bracket;
Run;

proc gchart data = census;
title "Percentage of People in Certain Income Thresholds by Race";
pie Race / percent = inside;
by Income_Bracket;
Run;

proc gchart data = census;
pie Occupation / percent = inside;
by Income_Bracket;
Run;

proc gchart data = census;
pie Workclass / percent = inside;
by Income_Bracket;
Run;

proc sort data = census;
by Workclass;
Run;

* Pie Charts ;
proc gchart data = census;
title "Percent of people in private sector in each income bracket";
pie income_bracket / percent=inside;
by Workclass;
run;

* Correlation and Regression ;
proc corr data = census;
var hours_per_week age capital_gains capital_losses education_num num_income;
run;

proc gchart data = census;
pie Occupation / percent = inside;
by Workclass;
Run;

