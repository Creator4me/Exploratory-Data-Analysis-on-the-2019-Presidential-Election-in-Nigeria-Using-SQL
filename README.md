# Exploratory-Data-Analysis-on-the-2019-Presidential-Election-in-Nigeria-Using-SQL
### Projet Overview
The provided queries analyze the 2019 Presidential Election in Nigeria. Various aspects of the election data have been explored to understand voter turnout, party performances, regional patterns, and more.
### Data Source
The primary dataset utilized for this investigative analysis is the "2019election.csv" file, which details the electoral performance of the dominant political parties in the 2019 presidential election.
### Tool Used

-Excel - Data Cleaning 
  - [Download Here](https://microsoft.com)
    
-SQL - Data Analysis

-PowerBi - Creating Report
 - [Download Here](https://powerbi.microsoft.com)

### Data Cleaning And Preparation
To guarantee the dataset's cleanliness, the subsequent tasks were executed.

1.Data Loading and Inspection

2.Data Cleaning and Formatting

### Exploratory Data Analysis(EDA)

This involves exploring the election data to answer key questions such as:

- Count of States?
- Count of Geopolitical Zones?
- Spread of Potential Voters?
- Spread of Accredited Voters?
- Spread of Cast Votes?
- Spread of Valid Cast Votes?
- Spread of Rejected Votes?
- Voting Trends by Region?
- Voter Participation Rate?
- Which state holds the highest Percentage of PDP Votes?
- Which state holds the highest Percentage of APC Votes?
- Difference in Votes Margin between the Parties?
- What Percentage of Rejected Votes is seen Across States?
- Trend in Geopolitical Zones?

### Data Analysis
  Code utilized/Features addressed
```SQL

  --To get the entire Table--
SELECT *
FROM Election_2019;

--Exploratory Data Analysis On the 2015 Presidential Election In Nigeria--
--Number Of States--
SELECT COUNT(state)
FROM Election_2019;

--Number Of Geopolitical Zones
SELECT COUNT(distinct geopolitical_zone)
FROM Election_2019;

--Distribution of Potential Voter Across The States in 2019
SELECT state,num_of_reg_voters as potential_Voters
FROM Election_2019
ORDER BY num_of_reg_voters DESC;


--Distribution of Accredited Voters in the 2019 Election 
SELECT state,accredited_voters
FROM Election_2019
ORDER BY accredited_voters DESC;

---Distribution of Votes Cast Across The States in the 2019 Election
SELECT state,votes_cast
FROM Election_2019
ORDER BY  votes_cast DESC;

--Distribution of Valid Votes Cast Across The States in the 2019 Election
select state,valid_votes
from Election_2019
order by valid_votes desc;

--Distribution of Rejected Votes Cast Across The States in the 2019 Election
SELECT state,rejected_votes
FROM Election_2019
ORDER BY rejected_votes DESC;

--Regional Voting Patterns--
SELECT geopolitical_zone,
       SUM(pdp_votes) as total_pdp_votes,
	   SUM(apc_votes)as total_apc_votes,
	   ROUND(SUM(pdp_votes) *100.0/SUM(valid_votes),2) as pdp_percentage,
	   ROUND(SUM(apc_votes) *100.0/SUM(valid_votes),2) as apc_percentage
FROM Election_2019
GROUP BY  geopolitical_zone
ORDER BY geopolitical_zone;

--Voter Turnout Rate--
SELECT state,(accredited_voters::FLOAT/num_of_reg_voters)* 100 as turnout_rate
FROM Election_2019
ORDER BY turnout_rate;

--State With Highest Percentage Of Vote in PDP--
SELECT state, pct_of_pdp_votes
FROM Election_2019
ORDER BY pct_of_pdp_votes DESC
limit 1;

--State With Highest Percentage Of Vote in APC--
SELECT state, pct_of_apc_votes
FROM Election_2019
ORDER BY pct_of_apc_votes DESC
limit 1;

--Marginal Difference Of Votes Betwen Both Parties--Threshold 10%
SELECT state, ABS(pct_of_pdp_votes - pct_of_apc_votes) AS vote_difference
FROM Election_2019
WHERE ABS(pct_of_pdp_votes - pct_of_apc_votes) < 10 -- Adjust the threshold (5 in this case) for the maximum acceptable difference
ORDER BY vote_difference;

--Marginal Difference Of Votes Betwen Both Parties--Threshold 5%
SELECT state, ABS(pct_of_pdp_votes - pct_of_apc_votes) AS vote_difference
FROM Election_2019
WHERE ABS(pct_of_pdp_votes - pct_of_apc_votes) < 5 -- Adjust the threshold (5 in this case) for the maximum acceptable difference
ORDER BY vote_difference;

--Percentage Of Rejected Votes Across The States
SELECT state,
       (rejected_votes::FLOAT / votes_cast) * 100.0 AS pct_rejected_votes
FROM Election_2019;

--Geopolitical Zone Trend--
SELECT geopolitical_zone,
       CASE
           WHEN MAX(pdp_votes) > MAX(apc_votes) THEN 'PDP'
           WHEN MAX(apc_votes) > MAX(pdp_votes) THEN 'APC'
           ELSE 'Equal Dominance'  -- If votes for PDP and APC are equal
       END AS predominant_party
FROM Election_2019
GROUP BY geopolitical_zone;
```
### Outcome/Discoveries

The summary of the analysis findings is as follows:

1.Regarding potential voter distribution among states, Lagos State stood out with the highest count with 6.3 million potential Voters, while
Ekiti with 899919 thousand potential voters had the lowest among the 37 states.

2.Examining accredited voters, Kano recorded the highest numbers with over 2 million, while Bayelsa had the lowest counts at just above 300 thousand

3.Kano and Kaduna had significant valid votes at 1.89 million and 1.66 million respectively, whereas Bayelsa,Ebonyi,Abia and Ekiti had notably lower counts at over 300 thousand

4.Kano had the highest count of rejected votes at 74 thousand , while Ekiti had the lowest at 13 thousand.

5.In terms of regional voting patterns, PDP secured more votes in the South-South with 2 million votes, while APC dominated in the Northwest with 6 million and Northeast with 3
million votes but the margin of votes in the Northcentral and Southwest were close

6.Voter turnout was notably higher in Jigawa State at 57% compared to Lagos State and Abia State at 20% and 19%

7.Ogun State and Enugu State had the highest percentage of rejected votes at 6.7%, while states like osun and plateau had significantly lower percentage of rejected Votes.

8.PDP garnered more votes in several states such as Anambra, Rivers, Akwa Ibom, Abia, Enugu, and Bayelsa. APC performed better in states like Borno, Yobe, Bauchi, Kano, and Jigawa.

9.Anambra State had the highest percentage of PDP Votes while Borno had the highest percentage of APC Votes

### Recommendation
 Based on the analysis findings, I recommend the following actions:
  - Improve voter education in states experiencing higher rates of rejected votes, focusing on educating citizens about the voting procedures.
  - Increase awareness in states with lower voter turnout, emphasizing the significance of active participation in elections.

### Limitation
 - We omitted records associated with other parties to emphasize our attention on the two primary political entities.

### References
 - The Dataset used for this analysis was obained from Independent National Electoral Commission
   - [Download here](https://www.inecnigeria.org)


   

