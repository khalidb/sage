


Create table v_lineitem as select * from LINEITEM where L_SHIPDATE >= '1997-01-01';

Query.

Select * 
From part, partsupp, supplier, v_lineitem, n_orders, nation
Where (s_suppkey = l_suppkey) and (ps_suppkey = l_suppkey) and (ps_partkey = l_partkey) and (p_partkey = l_partkey) and (o_orderkey = l_orderkey) and (s_nationkey = n_nationkey) and (p_name not like '%dark%');

Count = 15105


-- Auxiliary queries that are used for building the witness matrix



.mode csvs
.header off

.output ./w_part.csv
Select id from (
Select * 
From part_rp, partsupp, supplier, v_lineitem, n_orders, nation
Where (s_suppkey = l_suppkey) and (ps_suppkey = l_suppkey) and (ps_partkey = l_partkey) and (p_partkey = l_partkey) and (o_orderkey = l_orderkey) and (s_nationkey = n_nationkey) and (p_name not like '%dark%')
);

.output ./w_partsupp.csv
Select id from (
Select * 
From part, partsupp_rp, supplier, v_lineitem, n_orders, nation
Where (s_suppkey = l_suppkey) and (ps_suppkey = l_suppkey) and (ps_partkey = l_partkey) and (p_partkey = l_partkey) and (o_orderkey = l_orderkey) and (s_nationkey = n_nationkey) and (p_name not like '%dark%')
);

.output ./w_supplier.csv
Select id from (
Select * 
From part, partsupp, supplier_rp, v_lineitem, n_orders, nation
Where (s_suppkey = l_suppkey) and (ps_suppkey = l_suppkey) and (ps_partkey = l_partkey) and (p_partkey = l_partkey) and (o_orderkey = l_orderkey) and (s_nationkey = n_nationkey) and (p_name not like '%dark%')
);

.output ./w_lineitem.csv
Select id from (
Select * 
From part, partsupp, supplier, lineitem_rp, n_orders, nation
Where (s_suppkey = l_suppkey) and (ps_suppkey = l_suppkey) and (ps_partkey = l_partkey) and (p_partkey = l_partkey) and (o_orderkey = l_orderkey) and (s_nationkey = n_nationkey) and (p_name not like '%dark%')
);


.output ./w_orders.csv
Select id from (
Select * 
From part, partsupp, supplier, v_lineitem, orders_rp, nation
Where (s_suppkey = l_suppkey) and (ps_suppkey = l_suppkey) and (ps_partkey = l_partkey) and (p_partkey = l_partkey) and (o_orderkey = l_orderkey) and (s_nationkey = n_nationkey) and (p_name not like '%dark%')
);

.output ./w_nation.csv
Select id from (
Select * 
From part, partsupp, supplier, v_lineitem, n_orders, nation_rp
Where (s_suppkey = l_suppkey) and (ps_suppkey = l_suppkey) and (ps_partkey = l_partkey) and (p_partkey = l_partkey) and (o_orderkey = l_orderkey) and (s_nationkey = n_nationkey) and (p_name not like '%dark%')
);

.mode csv
.header on
.output ./results.csv
Select * 
From part, partsupp, supplier, v_lineitem, n_orders, nation
Where (s_suppkey = l_suppkey) and (ps_suppkey = l_suppkey) and (ps_partkey = l_partkey) and (p_partkey = l_partkey) and (o_orderkey = l_orderkey) and (s_nationkey = n_nationkey) and (p_name not like '%dark%');

.output stdout


