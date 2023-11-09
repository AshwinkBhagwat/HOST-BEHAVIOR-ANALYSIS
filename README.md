
# Project Title

Host-Behavior-Analysis
"Host-Behavior-Analysis" is a project focused on the analysis of property listings for a property rental company. The dataset provided has the information regarding the 2 cities in Canada, Greece, Italy, China and US each. The thorough analysis of the property listings is being done for the data and the meaningful insights have been provided, to comprehend how host behavior varies across a variety of metrics. Also extensive charts have been created to visualize the data in the best possible way.


## Problem Statement
The problem is to conduct a behavioral analysis of hosts for a property rental company to gain insights on the variation of host behavior across different metrics and identify areas for improvement.
## Insights
1.Response rate: -super hosts surpass the overall average 
response rate, with 98.31% compare to regular hosts
having avg rate is 90.75%, highlighting super hosts the commitment to exceptional service.

2.Acceptance rate: - 84% of super hosts surpass the overall average acceptance rate, but only 53% regular hosts
surpass avg.

3.Instant booking: - super hosts have a higher instant booking rate (54%) than regular hosts (39%), leading to
higher satisfaction, occupancy rates, repeat bookings, and success.

4.Identity verified: - super hosts have a higher verified profile rate (91%) than regular hosts (82%), indicating
a greater commitment to guest security and trust.
## SQL Queries

select * from All_listing;

/*a. Analyze different metrics to draw the distinction between Super Host and Other
Hosts:

use Rental_Property;

/*Total No. of Host and Superhost*/
select host_country,host_city,case
when host_is_superhost='TRUE' then 'SuperHost'
when host_is_superhost='FALSE' then 'Host' 
end as 
host_superhost,No_of_host from
(select host_is_superhost,host_country,host_city,count(*) as No_of_host from all_host 
group by host_is_superhost,host_country,host_city)a
order by host_country,host_city,host_is_superhost;

EXEC sp_help all_host;

/*Response Rate*/
select case
when host_is_superhost = 'TRUE' then 'SuperHost'
when host_is_superhost = 'FALSE' then 'Host' end as
host_Superhost,host_country,host_city,AvgResponseRate,MinResponseRate,MaxResponseRate from
(select host_is_superhost,host_country,host_city,avg(host_response_rate) as AvgResponseRate, min
(host_response_rate) as MinResponseRate , max(host_response_rate) as maxResponseRate
from all_host 
group by host_is_superhost,host_country,host_city)c
order by host_country,host_city;


/*No of host having response rate > avg(response rate)*/
select case
when host_is_superhost = 'TRUE' then 'SuperHost'
when host_is_superhost = 'FALSE' then 'Host' end as
host_Superhost,host_country,host_city,No_Of_Host from
(select host_is_superhost,host_country,host_city,count(host_id) as No_OF_Host from all_host where
host_response_rate > (select avg(host_response_rate) as Avg_Response_rate from all_host) 
group by host_is_superhost,host_country,host_city)c


/*Listing Count*/
select case
when host_is_superhost = 'TRUE' then 'SuperHost'
when host_is_superhost = 'FALSE' then 'Host' 
end as host_Superhost,host_country,host_city,AvgListingCount,MinListingCount,MaxListingCount from
(select host_is_superhost,host_country,host_city,avg(host_listings_count)as Avglistingcount,
min(host_listings_count) as Minlistingcount,
max(host_listings_count) as Maxlistingcount
from all_host 
group by host_is_superhost,host_country,host_city) aa


/*Listing Count > Avg. listing count*/
select case
when host_is_superhost = 'TRUE' then 'SuperHost'
when host_is_superhost = 'FALSE' then 'Host' 
end as
host_Superhost,host_country,host_city,No_Of_Host from
(select host_is_superhost,host_country,host_city,count(host_id) as No_OF_Host from all_host 
where host_listings_count >
(select avg(host_listings_count) as Avg_Listing_rate from all_host) group by
host_is_superhost,host_country,host_city)c;



/*Response Time*/
select case
when host_is_superhost = 'TRUE' then 'SuperHost'
when host_is_superhost = 'FALSE' then 'Host' 
end as
host_Superhost,host_country,host_city,host_response_time,TotalHost from
(select host_is_superhost,host_country,host_city,host_response_time,count(host_id) as TotalHost from all_host 
group by host_is_superhost,host_country,host_city,host_response_time ) ccc 
order by host_Superhost,host_country,host_city


/*Acceptance Rate*/
select case
when host_is_superhost = 'TRUE' then 'SuperHost'
when host_is_superhost = 'FALSE' then 'Host' 
end as
host_Superhost,host_country,host_city,AvgAcceptanceRate,MinAcceptanceRate,MaxAcceptanceRate from
(select host_is_superhost,host_country,host_city,avg(host_acceptance_rate) as
AvgAcceptanceRate,min(host_acceptance_rate) as MinAcceptanceRate,
max(host_acceptance_rate) as MaxAcceptanceRate from all_host 
group by host_is_superhost,host_country,host_city)c


/*No of host having acceptance rate > avg(acceptance rate)-*/
select case
when host_is_superhost = 'TRUE' then 'SuperHost'
when host_is_superhost = 'FALSE' then 'Host' 
end as 
host_Superhost,host_country,host_city,No_Of_Host from
(select host_is_superhost,host_country,host_city, count(host_id) as No_OF_Host from all_host 
where host_acceptance_rate > (select avg(host_acceptance_rate) as Avg_Acceptance_rate from all_host) 
group by host_is_superhost,host_country,host_city)c


/*profile Pic-*/
select case
when host_is_superhost = 'TRUE' then 'SuperHost'
when host_is_superhost = 'FALSE' then 'Host' 
end as
host_Superhost,host_country,host_city,host_has_profile_pic,TotalHost from
(select host_is_superhost,host_country,host_city,host_has_profile_pic,count(host_id) as TotalHost from
all_host
group by host_is_superhost,host_country,host_city,host_has_profile_pic ) ccc 
order by host_Superhost,host_country,host_city


/*Identity Verified*/
select case
when host_is_superhost = 'TRUE' then 'SuperHost'
when host_is_superhost = 'FALSE' then 'Host' 
end as host_Superhost,
case 
when host_identity_verified = 'FALSE' then 'NO'
when host_identity_verified = 'TRUE' then 'YES'
end as host_identity_verified,host_country,host_city,TotalHost from
(select host_is_superhost,host_country,host_city,host_identity_verified,count(host_id) as TotalHost from all_host
group by host_is_superhost,host_country,host_city,host_identity_verified ) ccc 
order by host_Superhost,host_country,host_city


/*Instant Booking*/
select case 
when host_is_superhost = 'TRUE' then 'SuperHost'
when host_is_superhost = 'FALSE' then 'Host' 
end as host_Superhost,host_country,host_city,NO_of_host from
(select b.host_is_superhost,host_country,host_city,count(distinct a.host_id) as NO_of_host from
All_listing a inner join all_host b on a.host_id = b.host_id
where instant_bookable = 'TRUE' 
group by b.host_is_superhost,host_country,host_city)c



/*b.Using the above analysis, identify the top 3 crucial metrics one needs to maintain
to become a Super Host and also, find their average values.*/
/*From the Above Analysis The Top 3 Crucial Metrics Are*/
1.Response Rate
2.Response Time
3.Acceptance Rate

--------

/*c. Analyze how the comments of reviewers vary for listings of Super Hosts vs Other
Hosts
(Extract words from the comments provided by the reviewers)*/

select case
when host_is_superhost = 'TRUE' then 'SuperHost'
when host_is_superhost = 'FALSE' then 'Host' 
end as host_Superhost,host_country,host_city,
No_of_Positive_comment from
(select host_is_superhost,host_country,host_city,sum(case when
comments like '%great%' or comments like '%nice%' or comments like '%wonderful%' or
comments like '%brilliant%'or
comments like '%great location%' or comments like '%good%' or comments like '%lovely%'
or comments like '%friendly%'or
comments like '%perfect%' or comments like '%beautiful%' or comments like '%definetly
stay%' or comments like '%excellent%'
or comments like '%highly recommended%' or comments like '%convenient%' or comments like '%clean%' 
or comments like '%loved%'  or comments like '%fantastic%' or comments like '%fabulous%' 
then 1 else 0 end) as No_of_Positive_comment from
(select a.comments,host_country,host_city,c.host_is_superhost from All_reviews a
inner join All_listing b on a.listing_id=b.id
inner join All_host c on b.host_id = c.host_id)dd
group by host_is_superhost,host_country,host_city)ee



select host_is_superhost,count(a.id) as Total_comments from All_reviews a
inner join All_listing b on a.listing_id=b.id
inner join All_host c on b.host_id = c.host_id
group by host_is_superhost



/*d. Analyze do Super Hosts tend to have large property types as compared to Other
Hosts*/
/*Large property Type*/

select case 
when host_is_superhost = 'FALSE' then 'Host'
when host_is_superhost = 'TRUE' then 'SuperHost' 
end as host_Superhost,host_country,host_city,No_Of_property from
(select host_is_superhost,host_country,host_city , count(property_type) as No_Of_Property from
(select a.property_type,host_country,host_city,b.host_is_superhost from all_listing a inner join All_host b on a.host_id=b.host_id
where a.property_type like '%entire%' or
a.property_type like '%RV%' or a.property_type like '%Private%' or a.property_type
like '%Shared%' or a.property_type like '%Room%'
or a.property_type like '%Cycladic%' or a.property_type like '%Boat%' or
a.property_type like '%Floor%' or a.property_type like '%Tiny%'
or a.property_type like '%Earth%')c
group by host_is_superhost,host_country,host_city ) cc;

select property_type,count(property_type) as cnt from all_listing
group by property_type
order by cnt desc;


/*Review*/
select host_country,host_city,avg(a.review_scores_rating) as review_scores_rating ,b.host_is_superhost
from all_listing a inner join all_host b on a.host_id = b.host_id
group by b.host_is_superhost,host_country,host_city
## 🛠 Skills
Python , SQL SEVER , POWER BI , EXCEL


