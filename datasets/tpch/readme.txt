

.mode csv
.header on
.import customer.csv customer
.import supplier.csv supplier
.import view_lineitem.csv lineitem
.import part.csv part
.import partsupp.csv parsupp
.import region.csv region
.import nation.csv nation
.import orders.csv orders


-- q2

.output q2.csv

select P_PARTKEY,P_MFGR,S_NAME,S_ADDRESS,S_PHONE,S_ACCTBAL,S_COMMENT,N_NAME
from part, supplier, partsupp, nation, region
where
p_partkey = ps_partkey
and s_suppkey = ps_suppkey 
and s_nationkey = n_nationkey and n_regionkey = r_regionkey;

-- q3

.output q3.csv

Select O_ORDERDATE,O_SHIPPRIORITY,L_ORDERKEY,L_EXTENDEDPRICE,L_DISCOUNT
From customer, orders, lineitem
Where (c_custkey = o_custkey) and (l_orderkey = o_orderkey);


-- q9

.output q9.csv

Select PS_SUPPLYCOST,L_QUANTITY,L_EXTENDEDPRICE,L_DISCOUNT,O_ORDERDATE,N_NAME 
From part, partsupp, supplier, lineitem, orders, nation
Where (s_suppkey = l_suppkey) and (ps_suppkey = l_suppkey) and (ps_partkey = l_partkey) and (p_partkey = l_partkey) and (o_orderkey = l_orderkey) and (s_nationkey = n_nationkey) and (p_name not like '%dark%');


-- q11

.output q11.csv

Select PS_PARTKEY,PS_AVAILQTY,PS_SUPPLYCOST
From supplier, partsupp, nation
Where (ps_suppkey = s_suppkey) and (s_nationkey = n_nationkey);

.output stdout





