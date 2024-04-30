# Swiggy-live-dashboard
Power bi swiggy Dashboard
used various measures code - 
1 Active_users = DISTINCTCOUNT(orders[user_id])
2 Dynamic_subHeading = "Swiggy providing services in "&COUNT(orders[city])&" and connected with "&DISTINCTCOUNT(users[user_id])&"
  where got "&count(orders[user_id])&" Orders."
3 Dynamic_TopN_Title = VAR selectedRank = SELECTEDVALUE(RankTable[Type])
                     VAR selectedType = SELECTEDVALUE(orders[Type])
RETURN
selectedRank&" city "&selectedType
4 gaincustomers = VAR FilterUsers = FILTER(SUMMARIZE(users,users[user_id]),AND([prevyrsale]<=0 , [curr_year]>=0))
RETURN
CALCULATE([user_count],FilterUsers)
5 Lostcustomers = VAR FilterUsers = FILTER(SUMMARIZE(users,users[user_id]),AND([curryrsale]<=0 , [prevyrsale]>=0))
RETURN
CALCULATE([user_count],FilterUsers)
6 prev_year = [curr_year] - 1
7 prevyrsale = VAR yr = [prev_year]
RETURN
CALCULATE([Sales_table],orders[Year]=yr)
8 Sales_table = SUM(orders[Value])
9 TopN_Sale = 
VAR Rankvalue = RANKX(ALL(orders[city]),[Sales_table],,DESC)
                VAR SELECTEDRank=SELECTEDVALUE(RankTable[No])
RETURN
IF(SELECTEDRank=0,[Sales_table],
IF(Rankvalue<=SELECTEDRank,[Sales_table],BLANK())
)
10 user_count = DISTINCTCOUNT(users[user_id])
11 Order_count = COUNT(orders[order_date])
12 Rating_count = COUNT(restaurant[rating])
13 Dynamic buttons - RankTable = DATATABLE("Sort", INTEGER, "Type", STRING, "No", INTEGER
                 ,{             {0,"All" ,0},
                                {1,"Top 5" ,5},
                                {2,"Top 10" ,10},
                                {3,"Top 20" ,20},
                                {4,"Top 50" ,50},
                                {5,"Top 100" ,100}

                 }
            )
