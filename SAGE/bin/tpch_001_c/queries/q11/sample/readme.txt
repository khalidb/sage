


Query.

Select * 
From supplier, partsupp, nation
Where (ps_suppkey = s_suppkey) and (s_nationkey = n_nationkey);



Count = 8000


-- Auxiliary queries that are used for building the witness matrix



.mode csvs
.header off

.output ./w_supplier.csv
Select id from (
Select * 
From supplier_rp, partsuppView, nation
Where (ps_suppkey = s_suppkey) and (s_nationkey = n_nationkey)
);

.output ./w_partsupp.csv
Select id from (
Select * 
From supplierView, partsupp_rp, nation
Where (ps_suppkey = s_suppkey) and (s_nationkey = n_nationkey)
);

.output ./w_nation.csv
Select id from (
Select * 
From supplierView, partsuppView, nation_rp
Where (ps_suppkey = s_suppkey) and (s_nationkey = n_nationkey)
);



.mode csv
.header on
.output ./results.csv
Select * 
From supplierView, partsuppView, nation
Where (ps_suppkey = s_suppkey) and (s_nationkey = n_nationkey);

.output stdout


