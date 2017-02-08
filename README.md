# election2016

See Codebook in /data/input/codebook_election.xlsx for variable definitions. 

For 2016 and 2012 election variables: 
Presidential results were obtained from the 3 election sources linked in the codebook
as well as  MS SoS website. Data accuracy was checked against mid-November 2016 preliminary results orginially hosted on
Townhall.com (but not included). 3rd party breakdown (Johnson, Stein, Other) is missing for Kansas and Maine. Data for 
the Kansas senate race are similarily missing.

For constructed variables (e.g. racial diversity), descriptive definitions are in the Codebook. In general, enough detail is
provided to allow for reproduduction.

Specifically for the 2012 Nytrank variable, The code to generate: 

## Nytrank http://nyti.ms/2k1WWFM
### Rank Vars
library("data.table")
dt <- dt[!is.na(unemp), u3rank := rank(unemp,ties.method = "min")] 
dt <- dt[!is.na(ed_degree), edrank := rank(-ed_degree, ties.method = "min")]
dt <- dt[!is.na(disability), disrank := rank(disability, ties.method = "min")]
dt <- dt[!is.na(le10), lerank := rank(-le10, ties.method = "min")]
dt <- dt[!is.na(hh_income12), mhirank := rank(-hh_income12, ties.method = "min")]
dt <- dt[!is.na(obese11), obeserank := rank(obese, ties.method = "min")]

### Replace NAs
dt$u3rank[is.na(dt$u3rank)] <- 0
#dt$edrank[is.na(dt$edrank)] <- 0
#dt$disrank[is.na(dt$disrank)] <- 0
#dt$lerank[is.na(dt$lerank)] <- 0
dt$mhirank[is.na(dt$mhirank)] <- 0
#dt$obeserank[is.na(dt$obeserank)] <- 0

### Rank Check
table(((as.numeric((dt$edrank!=0))) + (as.numeric((dt$u3rank!=0))) +
         (as.numeric((dt$disrank!=0))) + (as.numeric((dt$lerank!=0))) +
         (as.numeric((dt$mhirank!=0))) + (as.numeric((dt$obeserank!=0)))))

## Combine
dt$nytrank <- (dt$edrank + dt$u3rank + dt$disrank
                    + dt$lerank + dt$mhirank + dt$obeserank)/
  ((as.numeric((dt$edrank!=0))) + (as.numeric((dt$u3rank!=0))) +
     (as.numeric((dt$disrank!=0))) + (as.numeric((dt$lerank!=0))) +
     (as.numeric((dt$mhirank!=0))) + (as.numeric((dt$obeserank!=0)))
  )
  
  In addition to the codebook, two analysis files are provided to provide instructive examples of using the dataset. 
  A few rough plots of the data using mapinseconds.com are also provided.
