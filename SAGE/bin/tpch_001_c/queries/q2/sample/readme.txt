
Attribute that are used in the join or projected out for each relation:
Part: p_partkey, p_mfgr
Supplier: s_suppkey, s_nationkey, s_acctbal, s_name, s_address, s_phone, s_comment
Partsupp: ps_partkey, ps_suppkey 
Nation: n_nationkey, n_regionkey, n_name
Region: r_regionkey


select *
from part, supplierview, partsuppview, nation, region
where
p_partkey = ps_partkey
and s_suppkey = ps_suppkey 
and s_nationkey = n_nationkey and n_regionkey = r_regionkey;

-- Auxiliary queries that are used for building the witness matrix

Drop view region_rp;

create view region_rp as select rowid as id, * from region;
create view nation_rp as select rowid as id, * from nation;
create view partsupp_rp as select (rowid -1) as id, * from partsuppView;
create view supplier_rp as select (rowid -1) as id, * from supplierView;
create view part_rp as select (rowid -1) as id, * from partView;

.mode csv
.header off

.output ./w_region.csv
Select id from (select * from partView, supplierView, partsuppView, nation, region_rp
where
p_partkey = ps_partkey
and s_suppkey = ps_suppkey 
and s_nationkey = n_nationkey and n_regionkey = r_regionkey);

.output ./w_nation.csv
Select id from (select * from partview, supplierview, partsuppview, nation_rp, region
where
p_partkey = ps_partkey
and s_suppkey = ps_suppkey 
and s_nationkey = n_nationkey and n_regionkey = r_regionkey);

.output ./w_partsupp.csv
Select id from (select * from partview, supplierview, partsupp_rp, nation, region
where
p_partkey = ps_partkey
and s_suppkey = ps_suppkey 
and s_nationkey = n_nationkey and n_regionkey = r_regionkey);

.output ./w_supplier.csv
Select id from (select * from partview, supplier_rp, partsuppview, nation, region
where
p_partkey = ps_partkey
and s_suppkey = ps_suppkey 
and s_nationkey = n_nationkey and n_regionkey = r_regionkey);

.output ./w_part.csv
Select id from (select * from part_rp, supplierview, partsuppview, nation, region
where
p_partkey = ps_partkey
and s_suppkey = ps_suppkey 
and s_nationkey = n_nationkey and n_regionkey = r_regionkey);

.mode csv
.header on
.output ./results.csv
select *
from partview, supplierview, partsuppview, nation, region
where
p_partkey = ps_partkey
and s_suppkey = ps_suppkey 
and s_nationkey = n_nationkey and n_regionkey = r_regionkey;

.output stdout


