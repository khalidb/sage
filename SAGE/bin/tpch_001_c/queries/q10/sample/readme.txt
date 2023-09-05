


Create table v_lineitem as select * from LINEITEM where L_SHIPDATE >= '1997-01-01';

Query.

Select * 
From customer, n_orders, v_lineitem, nation
Where (c_custkey = o_custkey) and (o_orderkey = l_orderkey) and (c_nationkey = n_nationkey) and ((o_orderdate || '') like '1996%');


Count = 1552


-- Auxiliary queries that are used for building the witness matrix



.mode csvs
.header off

.output ./w_customer.csv
Select id from (
Select * 
From customer_rp, ordersView, view_lineitem_View, nation
Where (c_custkey = o_custkey) and (o_orderkey = l_orderkey) and (c_nationkey = n_nationkey) and ((o_orderdate || '') like '1996%')
);

.output ./w_orders.csv
Select id from (
Select * 
From customerView, orders_rp, view_lineitem_View, nation
Where (c_custkey = o_custkey) and (o_orderkey = l_orderkey) and (c_nationkey = n_nationkey) and ((o_orderdate || '') like '1996%')
);

.output ./w_lineitem.csv
Select id from (
Select * 
From customerView, ordersView, view_lineitem_rp, nation
Where (c_custkey = o_custkey) and (o_orderkey = l_orderkey) and (c_nationkey = n_nationkey) and ((o_orderdate || '') like '1996%')
);

.output ./w_nation.csv
Select id from (
Select * 
From customerView, ordersView, view_lineitem_View, nation_rp
Where (c_custkey = o_custkey) and (o_orderkey = l_orderkey) and (c_nationkey = n_nationkey) and ((o_orderdate || '') like '1996%')
);


.mode csv
.header on
.output ./results.csv
Select * 
From customerView, ordersView, view_lineitem_View, nation
Where (c_custkey = o_custkey) and (o_orderkey = l_orderkey) and (c_nationkey = n_nationkey) and ((o_orderdate || '') like '1996%');

.output stdout


