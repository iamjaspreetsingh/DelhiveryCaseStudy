# Delhivery Case Study

## Understandings:
1. start_scan_to_end_scan is in minutes -> od_end_time - od_start_time
2. Same values after grouping - 'data','trip_creation_time' 'route_schedule_uuid', 'route_type',
'source_center', 'source_name', 'destination_center', 'destination_name', 'od_start_time','od_end_time',
'start_scan_to_end_scan'
3. Different values after grouping - is_cutoff, cutoff_factor, , 'cutoff_timestamp',
'actual_distance_to_destination', 'actual_time', 'osrm_time', 'osrm_distance', 'factor',
'segment_actual_time', 'segment_osrm_time', 'segment_osrm_distance','segment_factor'
4. cutoff_factor is distance that is supposed to be there for actual_distance_to_destination. For trip
starting, it is always False.
5. max(actual_distance_to_destination) gives total distance as it is cumulative column
6. max(actual_time) is time taken for covering as it is cumulative column.
7. Similarly for osrm distance & osrm time. These are cumulative columns so taking max
8. 'segment_actual_time', 'segment_osrm_time', 'segment_osrm_distance','segment_factor' are non
cumulative columns
9. factor = actual_time/osrm_time
10. segment_factor = segment_actual_time/segment_osrm_time
11. segment_distance_to_destination can be calculated using actual_distance_to_destination by
subtracting from next row values.
12. Note, we saw some error in values in segments aggregations.


## Business Insights & Recommendations
1. FTL has better speed, lesser time than Carting in mostly all cases. (We notice some errors in data where
speed of Carting is unreasonably high for few trips which should be error/outliers - handled in final_df).
2. Karnataka<->Karnataka is busiest corridor state (closely followed by Maharashtra<->Maharashtra) used
in 2323 trips out of 18,947 subtrips (~12%).
3. Delhi <-> Haryana is busiest interstate corridor (where carting is 6 times more used than FTL).
4. Carting is used for distances upto 100 kms in busiest corridor state
5. We can use carting for all corridor listed in corridor_bad_ftl (list printed above) to save both time and
money. (Avg time in carting for same corridor is lesser than in FTL)
6. Destination city for most of the trips is Bengaluru city (followed by Gurgaon) and source city is also
Bengaluru (followed by Gurgaon) for most trips. Bengaluru is the hub for most trips.
7. We notice that most trips whose source is Karnataka, destination state is also Karnataka (~90%). It is
true for vice-versa too.
8. Carting is preferred twice as much than FTL in Bengaluru corridor.
9. When Carting, mean difference between OSRM time & actual time is 4 times smaller (hence better) than
when using FTL.
10. In busiest corridor state (Karnataka), we see bad performing trips by FTL - speed<20, distance<100. So,
our recommendation would be to prefer using Carting for these sources & destinations. It may be
cheaper as well (and more aligned to OSRM times as well)
11. The next recommendation would be to prefer long distance when suggested by OSRM. It is so because
while analysing all the trips where deadline (cutoff_timestamp) was missed, we saw we took average
twice the time we would have taken if followed OSRM suggestion even though distance covered by
OSRM suggestion is 33% more average.

<img width="701" alt="Screenshot 2023-09-06 at 12 36 49 AM" src="https://github.com/iamjaspreetsingh/DelhiveryCaseStudy/assets/30948046/f2af9a16-1490-4c6b-8ac4-b3ebab5c2db7">
<img width="540" alt="Screenshot 2023-09-06 at 12 36 09 AM" src="https://github.com/iamjaspreetsingh/DelhiveryCaseStudy/assets/30948046/ae115a58-c855-4ffc-94ca-54503d80f3b3">
