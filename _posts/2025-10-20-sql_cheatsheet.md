---
title: SQLチートシート
description: Oracle運用保守用
categories: [SQL,Oracle,SQL,Cheatsheet]
image:  /assets/img/2025-10-21-sqlcheatsheet/sql_icon.png
tags: [sql,oracle,it]     # TAG names should always be lowercase
mermaid: true
---

<br>
<!-- no robot! -->
<meta name="robots" content="noindex">


<!-- border setting -->
<style>
hr {
  padding: 20px 0;
  overflow: visible;
}
.hr1 {
  border-top: 1px dashed #aaa;
}
.hr1::after {
  display: inline-block;
  position: relative;
  top: -35px;
  left: 50px;
  padding: 0 3px;
  background: #eeeeee;
  color: #aaa;
  font-size: 15px;
} 
</style>


<br>

## Ora-1652チェック
直近1時間の一時表領域のセッションごとの使用量チェックSQL

```sql
-- インスタンスチェック
select instance_name from v$instance;
alter session set nls_date_format='YYYY/MM/DD HH24:MI:SS';

-- 直近1時間の一時表領域の使用量
set pages 2000 lin 300 colsep '|' tab off
col SAMPLE_TIME for a18
col TEMP_SPACE_ALLOCATED for 9999999999
col USERNAME for a15
col MACHINE  for a30
col PROGRAM for a35
col EVENT for a30
prompt
prompt "↓↓↓ 直近1時間で100MB以上、一時表領域を表示します ↓↓↓"
prompt "---------------------------------------------"
prompt
prompt
select 
  TO_CHAR(sample_time, 'YYYY/MM/DD HH24:MI:SS') SAMPLE_TIME
  ,temp_space_allocated/1024/1024 "temp_space_allocated(MB)"
  ,username
  ,machine
  ,sql_id
  ,program
  ,session_id
  ,session_serial#
  .event
  ,session_state
from
  dba_hosts_active_sess_history dhas
  ,dba_users du
where
  dhas.user_id=du.user_id
and
  temp_space_allocated > 104857600
order by sample_time
;
```
<br><br>

### 一時表領域使用量確認
一時表領域使用量チェックSQL

```sql
-- インスタンスチェック
select instance_name from v$instance;
alter session set nls_date_format='YYYY/MM/DD HH24:MI:SS';

-- 一時表領域使用量チェック
set pages 3000 lin 300
col Name for a15
prompt
prompt "↓↓↓ 一時表領域使用量を表示します ↓↓↓"
prompt "---------------------------------------------"
prompt  
select 
  d.tablespace_name "Name"
  ,to_char(nvl(a.bytes/1024/1024,0),'99,999,990') "Size(MB)"
  ,to_char(nvl(t.hwm,0)/1024/1024,'99999999') "HWM(MB)"
  ,to_char(nvl(t.hwm/a.bytes * 100,0),'999.00') "Using(MB)"
  ,to_char(nvl(t.bytes/1024/1024,0),'99999999.00') "Using(MB)"
from 
  dba_tablespace d
  ,(select tablespace_name,sum(bytes) bytes from dba_temp_files group by tablespace_name) a
  ,(select tablespace_name, sum(bytes_cached) hwm, sum(bytes_used) bytes from v$temp_extent_pool group by tablespace_name) t
where
  d.tablespace_name = a.tablespace_name(+)
  and d.tablespacename = t.tablespace_name(+)
  and d.extent_management like '%LOCAL%'
  and d.contents like '%TEMPORARY%'
  ;
```
<br>

### SQL テキストチェック
SQLIDでSQLテキストチェック

```sql
-- SQLテキストチェック
set pages 1000 lin 300
set heading off
set verify off
set trimspool on
set feedback off
set termout off
set long 10000000
set longchunksize 1024

select 
  sql_text 
from 
  dba_hist_sqltext 
where
  sql_id ='sqlid'
;
```

<br>

## テーブルバックアップスクリプト
引数1にusername/password  
引数2にスキーマ名と、テーブル名を指定して実行  
複数テーブル、スキーマの時→　  
(OWNER = 'TEST1' and TABLE_NAME='TEST1') OR  
(OWNER = 'TEST2' and TABLE_NAME='TEST2')   

```ksh
####################################################################################
#
# Name: Create_Backup_Table.sh
# Note: 
# 1. If there are same table names, counting loop only displays the row count.
# 2. Backup Table has incremental number on its table name
# 3. Take a username/password as first argument
# 4. Take a owner.table_name as second arguments
# 5. Shoe backup table lists
#
####################################################################################

echo "
set serveroutput on 
set timing on
set pages 3000 lin 300
DECLARE
    tname varchar2(50);
    bktname varchar2(60);
    diffnum number;

    suffixnum number;
    suffix char(2);
    command varchar2(300);
    rowcount number; -- for row count
    judge char(2);

    CURSOR tcursor is
    select owner || '.' || table_name as tname
    , 'select count(*) from ' || owner || ' . ' || table_name as table_count
    , substr(to_cahr(sysdate, 'MMDD') || '_' || owner || '_' || table_name, 1,27) as bktname
    from dba_tables
    where
    ${2}
    ;


BEGIN
    -- Initialize suffix
    suffixnum := 1;

    FOR tdata in tcursor LOOP
    DBMS_OUTPUT.PUT_LINE('-');
    DBMS_OUTPUT.PUT_LINE('-');
    DBMS_OUTPUT.PUT_LINE('------------------------------------');
    DBMS_OUTPUT.PUT_LINE('Create Table Start!');
    DBMS_OUTPUT.PUT_LINE('------------------------------------');

    -- Process 0 Table Row Count
    EXECUTE IMMEDIATE tdata.table_count into rowcount;  

    -- Process 1 Check Bktname and Tname
    suffix := LPAD(suffixnum,2,'0');
    tname := tdata.tname;
    bktname := tdata.bktname || '_' || suffix;
    DBMS_OUTPUT.PUT_LINE('Table:      [ ' || taname || ' ]');
    DBMS_OUTPUT.PUT_LINE('BKUP Table  [ ' || bktname|| ' ]');
    
    -- Process 2 Make Create Table Sentence
    command := 'create table ' || '¥"' || ' tablespace DBA_BKUP as select * from ' || tname ;
    DBMS_OUTPUT.PUT_LINE('Command:     [' || command || ']');
    
    -- Process 3 Execute Create Table Sentence
    EXECUTE IMMEDIATE command;

    -- Process 4 Display Count
    diffnum :=to_number(SQL%ROWCOUNT) - to_number(rowcount);
    If diffnum = 0 then
         judge := 'OK';
    Else judge := 'NG';
    End If;

    DBMS_OUTPUT.PUT_LINE('Backup Result:  [' || judge || ' ] ==> Before: [' || rowcount || ' ] | After: [' || SQL%ROWCOUNT || ' ]');
    DBMS_OUTPUT.PUT_LINE('Table Diff:     [' || diffnum ||' ]');

    DBMS_OUTPUT.PUT_LINE('------------------------------------');
    DBMS_OUTPUT.PUT_LINE('Create Table End!');"
    DBMS_OUTPUT.PUT_LINE('------------------------------------');
    DBMS_OUTPUT.PUT_LINE('-');
    DBMS_OUTPUT.PUT_LINE('-');

    -- suffixnum count up
    suffixnum := suffixnum + 1;
    END LOOP;

    EXCEPTION
    WHEN too_,many_rows THEN
    DBMS_OUTPUT.PUT_LINE('too many rows');
    
    WHEN others THEN
    DBMS_OUTPUT.PUT_LINE('-');    
    DBMS_OUTPUT.PUT_LINE('-');    
    DBMS_OUTPUT.PUT_LINE('///////////////////////////////////////');    
    DBMS_OUTPUT.PUT_LINE('Errors: ' || SQLERRM );    
    DBMS_OUTPUT.PUT_LINE('///////////////////////////////////////');    
    DBMS_OUTPUT.PUT_LINE('-');    
    DBMS_OUTPUT.PUT_LINE('-');    

END;
/

-- After creation
PROMPT
PROMPT
PROMPT DBA_BKUP Tablespace

set heading on
col owner for a20
col table_name for a30
alter session set nls_date_format='YYYY/MM/DD HH24:MI:SS';

select 
    t.owner
    ,t.table_name
    ,o.created
from
    dba_tables t
    inner join dba_objects o on
    t.table_name = o.object_name
where
    t.tablespace_name = 'DBA_BKUP'
    and to_char(o.created, 'YYYY/MM/DD') = TO_CHAR(SYSDATE,'YYYY/MM/DD')
order by
    substr(t.table_name, -2);

PROMPT
exit;
" | sqlplus -s ${1} 
```