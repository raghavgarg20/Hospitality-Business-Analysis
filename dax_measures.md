**Dax Measures** 

Total Revenue = SUM(
                fact_bookings[revenue_realized])

WoW Revenue = 
var selv = IF(
    HASONEVALUE(dim_calendar[Weeknum]),
    SELECTEDVALUE(dim_calendar[Weeknum]),
    MAX(dim_calendar[Weeknum])
)

VAR revcw = CALCULATE([Total Revenue],
dim_calendar[Weeknum]= selv)

VAR revpw= CALCULATE(
    [Total Revenue],
    dim_calendar[Weeknum]= selv-1
)

return 
   IF( 
     ISBLANK(revpw) || ISBLANK(revcw),
     BLANK(),
     revcw - revpw
   )

WoW Revenue % = DIVIDE(
    [WoW Revenue],
    [Previous Week Revenue],
    0
)

ADR = DIVIDE(
    [Total Revenue],
    [Total Bookings],
    0
)

WoW ADR = 
 var selv = IF(
    HASONEVALUE(dim_calendar[Weeknum]),
    SELECTEDVALUE(dim_calendar[Weeknum]),
    MAX(dim_calendar[Weeknum])
)

VAR adrcw = CALCULATE([ADR],
dim_calendar[Weeknum]= selv)

VAR adrpw= CALCULATE(
    [ADR],
    dim_calendar[Weeknum]= selv-1
)

return 
   IF( 
     ISBLANK(adrpw) || ISBLANK(adrcw),
     BLANK(),
     adrcw- adrpw
   )

WoW ADR % = 
DIVIDE(
    [WoW ADR],
    [Previous Week ADR],
    0
)

Average lead Time = 
AVERAGE(
    fact_bookings[Lead Time]
)

Realization % = DIVIDE(
    [Total Checked Out],
    [Total Bookings],
    0
)

WoW Realization % = 
var selv = IF(
    HASONEVALUE(dim_calendar[Weeknum]),
    SELECTEDVALUE(dim_calendar[Weeknum]),
    MAX(dim_calendar[Weeknum])
)

VAR realizationcw = CALCULATE([Realization %],
dim_calendar[Weeknum]= selv)

VAR realizationpw= CALCULATE(
    [Realization %],
    dim_calendar[Weeknum]= selv-1
)

return 
   IF( 
     ISBLANK(realizationcw) || ISBLANK(realizationpw),
     BLANK(),
     realizationcw-realizationpw
   )

DSRN ( daily Sellable Room Nights ) = 
     DIVIDE(
        [Total Capacity],
        [No of Days],
        0
     )


DURN (Daily Utilized Room Nights ) = 
DIVIDE(
    [Total Checked Out],
    [No of Days],
    0
)

Occupancy % = 
DIVIDE(
    [Total Successful Bookings],
    [Total Capacity],
    0
)


No of Days = DATEDIFF(
    MIN(dim_calendar[Date]),
    MAX(dim_calendar[Date]),
    DAY
) +1

Revenue Loss = [Ideal Revenue] - [Total Revenue]

WoW Revenue Loss = 
var selv = IF(
    HASONEVALUE(dim_calendar[Weeknum]),
    SELECTEDVALUE(dim_calendar[Weeknum]),
    MAX(dim_calendar[Weeknum])
)

VAR revlosscw = CALCULATE([Revenue Loss],
dim_calendar[Weeknum]= selv)

VAR revlosspw= CALCULATE(
    [Revenue Loss],
    dim_calendar[Weeknum]= selv-1
)

return 
   IF( 
     ISBLANK(revlosspw) || ISBLANK(revlosscw),
     BLANK(),
     revlosscw- revlosspw
   )


WoW Lead Time = 
var selv = IF(
    HASONEVALUE(dim_calendar[Weeknum]),
    SELECTEDVALUE(dim_calendar[Weeknum]),
    MAX(dim_calendar[Weeknum])
)

VAR leadcw = CALCULATE([Average lead Time],
dim_calendar[Weeknum]= selv)

VAR leadpw= CALCULATE(
    [Average lead Time],
    dim_calendar[Weeknum]= selv-1
)

return 
   IF( 
     ISBLANK(leadpw) || ISBLANK(leadcw),
     BLANK(),
     leadcw - leadpw
   )