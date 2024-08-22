---Daily Disbursment----
select * from c_LoanDisbursement with(nolock) where disbursementdate='2024-08-22' and PassedBy is null order by createdon desc;

select * from c_LoanDisbursement with(nolock) where disbursementdate='2024-08-22' and PassedBy is not  null order by createdon desc;

------Cnfrm For Disbursment----
 
select count(AccountId) from b_Account with (nolock) where AccountStatusCode='8' and Status='6057' 
and AccountOpenDate>='2024-03-15';


---TOTAL DISBURSMENT AMOUNT---
SELECT SUM(DisbAmount) AS TotalDisbursement
FROM c_LoanDisbursement with(nolock) where disbursementdate='2024-06-29' and PassedBy is not null;

----TOTAL DISBURSMENT AMOUNT (Monthly)------          
SELECT SUM(DisbAmount) AS TotalDisbursement
FROM c_LoanDisbursement with(nolock) where disbursementdate between('01-March-2024') and ('21-March-2024');

---Total Disabursment daywise with  amount---

SELECT DisbursementDate, COUNT(*) AS DisbursementCount, SUM(DisbAmount) AS TotalDisbursementAmount
FROM c_LoanDisbursement WITH (NOLOCK)
WHERE DisbursementDate >= '2024-02-01'
GROUP BY DisbursementDate order by DisbursementDate desc;


--- RTGS/NEFT Export---

select * from x_rtgsexportfile with(nolock) where createdon>'2024-08-22'

select * from x_RTGSExport with (NOLOCK) where RtgsExportFileId='12312'

select * from x_RTGSExportItems with (NOLOCK) where RtgsExportFileId='10211';

select * from x_RTGSDetail with (NOLOCK) where AccountId='2407023'

---GROUP INFO---
select *from g_group where groupid=228626

--MEMBER INFO---
select isactive,*from g_globalclient where globalclientid=62042

--Group Member--

select *from g_clientgroupmap where  groupid=405329
------------------

-----Appraisal--------

select top 20 * from b_accountappraisal

--------Grading Date Update----------

select GradingDate,* from b_ItemFormatEntry with (nolock)  where GroupId='228853'



---------Insurance Slab----
select * from b_insurancePremiumSlab

--------------
--DLJLGDetails---
select * from b_dljlgdetails where globalclientid=1688203
-----------BDC-Batch Details.------------

select * from b_AccDisbBatch order by CreatedOn desc;

select * from b_AccDisbBatchDetail with (NOLOCK) where AccDisbBatchId='58898';


select * from p_scheme--- For Finding ProducrID
select *from g_orgelement where OrgElementId=44

---
select *from h_employee---FSO---bank staff
select *from g_ngo---agent

--Account_Details, Loan_AC/Details, Loan_Repayment_Schedule (Amoritization Chart)--
select * from b_account where AccountId='1807423'
select * from c_loan where accountid=1190838
select * from c_repaymentschedule where accountid=1190838

--Duplicate_Aadhar Issue--
select *from g_clientdocmap where docrefno='775573415281'
select groupid,* from g_globalclient where GlobalClientId=1688203

--CRIF_API_Details--
 select * from g_CRIFAPIDataDetail with(nolock) where CRIFAPIDataId in(462)

--KYC Not Completed Issue--
select * from g_group ----(Check no of member in group)
select * from g_ClientGroupMap where groupid=120109

-- DELETE ANYWHERE BATCH TRANSACTION--
select  * from x_scroll with (nolock) where scrollnumber='716' and ScrollDate='20-Jan-2024';

--ScrollId- 4321520---- Get from "x_scroll" table (from first query)

select * from x_AnywhereTransactionBatch where ScrollId='4803033' and PassFlag='0'

select * from x_AnywhereTransactionBatch order by CreatedOn desc;

------DELETE from x_AnywhereTransactionBatch where ScrollId='' and PassFlag='0'

-- How to find a Table which content specific column--


SELECT t.name AS TableName, c.name AS ColumnName
FROM sys.tables t
INNER JOIN sys.columns c ON t.object_id = c.object_id
WHERE c.name like '%GlobalclientIncExpId%'

----Active User ata Time----

select Count(distinct(UserId)) from b_UserLogTime where IsOffline=0

------Remitance Related ISSUE--
select * from x_loanrepmembers where AccountId='137172'

select ReceiptNumber,* from x_loanRepayment where loanRepaymentID='5099633'

select * from x_LoanRepayRem where LoanRepayRemId='1273118'

select * from x_LoanRepayRemdetail where loanRepaymentID='8296939'
----------------------------------------------------------------------------------
--How to Find Stored Procedure----

SELECT name FROM sys.procedures WHERE 
type_desc = 'SQL_STORED_PROCEDURE' and name like '%ngoCOLLECTION%'

-------Reports in Que-----------------------------------------------

select * from sys_processbuffer with (NOLOCK) where statuschangedat>'22-Aug-2024' and status in(1,2);
select * from sys_ProcessBufferParam where ProcessBufferId='451885'
select * from sys_processbuffer where statuschangedat>'22-Aug-2024' and status in(3,4)  order by EndTimeStamp desc;



select * from a_user where userid in (1,2)


------------------------------------------,--
---USER STAFF NUMBER/DESIGNATION/EMPID----
select A.UserId,A.LoginName,A.EmployeeId,B.StaffNumber,B.FName,B.LName,D.OrgElementId,A.Status,A.Level,D.Name,C.DesignationId,C.DesignationType,C.Code,C.Name from a_User as A
inner join h_Employee as B ON A.EmployeeId=B.EmployeeId
inner join g_Designation as C ON B.DesignationId=C.DesignationId
inner join g_OrgElement as D ON B.OrgElementId=D.OrgElementId
WHere A.LoginName='1785 ' 

select A.UserId,A.LoginName,A.EmployeeId,B.StaffNumber,B.FName,B.LName,Case
When A.status=1 Then 'Active' Else 'Inactive' end as 'status',D.OrgElementId,D.Name,C.DesignationId,C.DesignationType,C.Code,C.Name from a_User as A
inner join h_Employee as B ON A.EmployeeId=B.EmployeeId
inner join g_Designation as C ON B.DesignationId=C.DesignationId
inner join g_OrgElement as D ON B.OrgElementId=D.OrgElementId
WHere A.UserId='4454'




---Which user has put report in q
SELECT pb.*
FROM sys_processbuffer pb
INNER JOIN sys_ProcessBufferParam pbp ON pb.ProcessBufferId = pbp.ProcessBufferId
WHERE pb.statuschangedat >= '2024-07-22' AND pb.status IN (1,2,3,4)
AND pbp.ParamName = '@p_userid' AND pbp.ParamValue = '3497';

----------Member Transfer----------
select * from g_grouptransferlog with (nolock)  where GlobalClientId in (2322861);

---Passing----
select * from g_TransferClient where TransferClientId=33583


select * from b_Account with (nolock) where groupid='419664'

select * from g_State 

select * from g_clientgroupmap where globalclientid=2210860



select * from c_repaymentschedule where accountid in (select accountid from b_account where Accountnumberfordisplay='338920134000404');

select IPFees,ProcessingFee,* from c_Loan where accountid in (select accountid from b_account where Accountnumberfordisplay='338920134000404');


select IPFees,ProcessingFee,LoanAmount,* from c_Loan where DisbursementAdviceDate>='2024-01-08' and LoanSchemeId='13'

-------------------------------------

select * from g_CRIFAPIData with (nolock)where CreatedOn>='2024-01-29'

select Status,* from b_Account where AccountNumberForDisplay='310320134008746'

select Status,LastModifiedOn,* from b_DLJLGDetails with (nolock) where GlobalClientId='3592844'

select * from c_RepaymentSchedule with (nolock) where AccountId='2385492'


select * from h_Employee where OrgElementId=3 and FName like '%j%'


select * from b_DLJLGDetails with (nolock) where GroupId='1920619'

select * from g_ClientGroupMap with (nolock) where GroupId='1920619'

select * from g_Group with (nolock) where GroupId='1920619'


select * from b_GradingHistory with (nolock) where GroupId='228853'

select CreatedOn,LastModifiedOn,LastModifiedBy,* from g_Group with (nolock) where GroupId='228853'

select Status,* from b_Account with (nolock) where groupid='228853'



select GradingDate,* from b_ItemFormatEntry with (nolock)  where GroupId='228853'

select count(Clientid) ,ClientOpDate from g_globalclient with (nolock) where createdon>='2024-02-15' group by ClientOpDate order by ClientOpDate desc;

---------------


SELECT DisbursementDate, COUNT(*) AS DisbursementCount, SUM(DisbAmount) AS TotalDisbursementAmount
FROM c_LoanDisbursement WITH (NOLOCK)
WHERE DisbursementDate >= '2024-03-10'
GROUP BY DisbursementDate order by DisbursementDate desc;

select DisbursementDate,COUNT (*) from c_LoanDisbursement with (nolock) where DisbursementDate >= '2024-02-25' group by DisbursementDate;

--------

select Status,* from b_DLJLGDetails with (nolock) where GroupId='474598';

select Status,GroupId,CreatedBy,* from b_DLJLGDetails with (nolock) where GlobalClientId in (3576544,3576519,3576508,3576562);

----
exec pr_ADHOC_NABFINS_BankBalance @p_Asondate='31-MARCH-2024';


select * from a_User where UserId='411'


select * from b_BranchList

select * from x_loanRepayment with (nolock) where TransactionDate>='2024-08-22' order by LastModifiedOn desc;
