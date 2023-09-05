


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
From part_rp, partsuppView, supplierView, view_lineitem_View, ordersView, nation
Where (s_suppkey = l_suppkey) and (ps_suppkey = l_suppkey) and (ps_partkey = l_partkey) and (p_partkey = l_partkey) and (o_orderkey = l_orderkey) and (s_nationkey = n_nationkey) 
);

.output ./w_partsupp.csv
Select id from (
Select * 
From part, partsupp_rp, supplierView, view_lineitem_View, ordersView, nation
Where (s_suppkey = l_suppkey) and (ps_suppkey = l_suppkey) and (ps_partkey = l_partkey) and (p_partkey = l_partkey) and (o_orderkey = l_orderkey) and (s_nationkey = n_nationkey)
);

.output ./w_supplier.csv
Select id from (
Select * 
From partView, partsuppView, supplier_rp, view_lineitem_View, ordersView, nation
Where (s_suppkey = l_suppkey) and (ps_suppkey = l_suppkey) and (ps_partkey = l_partkey) and (p_partkey = l_partkey) and (o_orderkey = l_orderkey) and (s_nationkey = n_nationkey) 
);

.output ./w_lineitem.csv
Select id from (
Select * 
From partView, partsuppView, supplierView, view_lineitem_rp, ordersView, nation
Where (s_suppkey = l_suppkey) and (ps_suppkey = l_suppkey) and (ps_partkey = l_partkey) and (p_partkey = l_partkey) and (o_orderkey = l_orderkey) and (s_nationkey = n_nationkey) 
);


.output ./w_orders.csv
Select id from (
Select * 
From partView, partsuppView, supplierView, view_lineitem_View, orders_rp, nation
Where (s_suppkey = l_suppkey) and (ps_suppkey = l_suppkey) and (ps_partkey = l_partkey) and (p_partkey = l_partkey) and (o_orderkey = l_orderkey) and (s_nationkey = n_nationkey) 
);

.output ./w_nation.csv
Select id from (
Select * 
From partView, partsuppView, supplierView, view_lineitem_View, ordersView, nation_rp
Where (s_suppkey = l_suppkey) and (ps_suppkey = l_suppkey) and (ps_partkey = l_partkey) and (p_partkey = l_partkey) and (o_orderkey = l_orderkey) and (s_nationkey = n_nationkey) 
);

.mode csv
.header on
.output ./results.csv
Select * 
From partView, partsuppView, supplierView, view_lineitem_View, ordersView, nation
Where (s_suppkey = l_suppkey) and (ps_suppkey = l_suppkey) and (ps_partkey = l_partkey) and (p_partkey = l_partkey) and (o_orderkey = l_orderkey) and (s_nationkey = n_nationkey) ;

.output stdout


