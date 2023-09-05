


Create table v_lineitem as select * from LINEITEM where L_SHIPDATE >= '1997-01-01';

Query.

Select * 
From customer, orders, v_lineitem
--Where (c_custkey = o_custkey) and (l_orderkey = o_orderkey) and (c_mktsegment = 'BUILDING');
Where (c_custkey = o_custkey) and (l_orderkey = o_orderkey);

select *
from part, supplier, partsupp, nation, region
where
p_partkey = ps_partkey
and s_suppkey = ps_suppkey 
and s_nationkey = n_nationkey and n_regionkey = r_regionkey;

-- Auxiliary queries that are used for building the witness matrix



insert into n_orders(rowid, O_ORDERKEY,O_CUSTKEY,O_ORDERSTATUS,O_TOTALPRICE,O_ORDERDATE,O_ORDERPRIORITY,O_CLERK,O_SHIPPRIORITY,O_COMMENT) SELECT row_number() OVER (ORDER BY rowid),  O_ORDERKEY,O_CUSTKEY,O_ORDERSTATUS,O_TOTALPRICE,O_ORDERDATE,O_ORDERPRIORITY,O_CLERK,O_SHIPPRIORITY,O_COMMENT from orders;

create view customer_rp as select (rowid -1) as id, * from customer;
create view orders_rp as select (rowid -1) as id, * from n_orders;
create view lineitem_rp as select (rowid -1) as id, * from v_lineitem;

.mode csvs
.header off

.output ./w_customer.csv
Select id from (Select * 
From customer_rp, ordersView, view_lineitem_View
Where (c_custkey = o_custkey) and (l_orderkey = o_orderkey) );

.output ./w_orders.csv
Select id from (Select * 
From customerView, orders_rp, view_lineitem_View
Where (c_custkey = o_custkey) and (l_orderkey = o_orderkey) );

.output ./w_lineitem.csv
Select id from (Select * 
From customerView, ordersView, view_lineitem_rp
Where (c_custkey = o_custkey) and (l_orderkey = o_orderkey) );


.mode csv
.header on
.output ./results.csv
select *
from partView, supplierView, partsuppView, nation, region
where
p_partkey = ps_partkey
and s_suppkey = ps_suppkey 
and s_nationkey = n_nationkey and n_regionkey = r_regionkey;

.output stdout


