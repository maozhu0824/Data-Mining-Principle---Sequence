# Data-Mining-Principle---Sequence
# Sequence procedure analyzes the ordering and timing of the relationship among multiple items. 
# First run the ASSOC procedure to create and output the data set of the assembled items. 
# In order to determine the timing of a rule, the SEQUENCE procedure uses a sequence variable, which serves as a time stamp for each observation. It can have numeric value, such as dates or times, which enable you to measure the time span from observation to observation. 
# Transactions that are missing sequence values are ignored entirely during the sequence computation. 

# The SEQUENCE Procedure: 
PROC SEQUENCE ASSOC = data-set=name    # output data set from the ASSOC procedure 
DATA = data-set-name     # identifies the input data source
DMDBCAT = catalog-name <options>; # NITEMS = number(maximum number of events that are possible for each rule); OUT = data-set-name; SUPPORT = number(minimum number of transactions that must occur for a rule to be accepted, rules that occur less than the number times are rejected, in addition to the SUPPORT in the ASSOC procedure, if not specified, it is 2% of the total transaction count.)

CUSTOMER variable; 
TARGET variable; 
VISIT variable </options>; # variable identifies time stamp, must contain a numeric value includes dates or times; SAME = same-number (specifies the lower time limit between two events you want to associate with each other, if occur less than this, then they will not be associated with each other and treated as occur at the same time. nonnegative and default is 0; WINDOW = window-number (upper time limit, if occur more than that, will not be considered for a rule count, default is maximum value possible for the input data set)

RUN;

# Example 
/* Run the DMDB Procedure */ 
proc dmdb batch data=sampsio.assocs    
dmdbcat=catSeq;    
id customer time;    
class product(desc); 
run; 

/* Run the ASSOC Procedure */ 
proc assoc data=sampsio.assocs    
dmdbcat=catSeq 
out=assocOut    
items=5 support=20;    
customer customer;
target product; 
run; 

/*Run the SEQUENCE Procedure */ 
proc sequence data=sampsio.assocs    
dmdbcat=catSeq 
assoc=assocOut    
out=seqOut 
nitems=3;    
customer customer;    
target product;    
visit time; 
run; 

/* Specifiy the Same and Windown Arguments */
/*Run the SEQUENCE Procedure */ 
proc sequence data=sampsio.assocs    
dmdbcat=catSeq 
assoc=assocOut    
out=seqOut2 
nitems=2    
support=25;    
customer customer;    
target product;   
visit time/same=1 window=2; 
run; 


