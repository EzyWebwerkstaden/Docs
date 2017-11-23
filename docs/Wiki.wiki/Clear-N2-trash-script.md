Om ni vill tömma trash på en sajt som kör N2, ändra @id till trash ID och kör på databasen.
Du kommer antagligen få felmeddelanden som säger något om "LinkValue" - Bry dig inte om dom, det funkar som det ska!
```
declare @id int
 
set @id = 4  -- 5
drop table #log
create table #log
(
       Id     int Identity not null,
       [statement]   varchar(5000),
       itemid int
)
drop table #result
create table #result
(
       Countid       int Identity not null,
       itemid int,
       parentId int,
       [level] int
)
 
drop table #detailCollectionIds
create table #detailCollectionIds
(
       Countid       int Identity not null,
       detailCollectionId   int
)
 
 
-- this CTE search for the (grand)children of the node
;WITH x (id, parent, [level])
AS
(
       select id, parentId, 1
       from [n2Item]
       where parentId = @id
       UNION ALL
       select t.id, t.parentId, x.level + 1
       from [n2Item] as t inner join
              x as x on x.id = t.parentId
)
insert into #result
select *
from x
 
-- display result
--select *
--from #result
--order by [level]
 
--drop table #t
--drop table #result
 
--select * from #result where itemid =34004 or parentid = 34004
 
 
declare @minid int
declare @maxId int
 
declare @minDetailCollectionId int
declare @maxDetailCollectionId int
 
declare @detailCollectionItemId int
declare @RealdetailCollectionId int
 
set @minid = (select MIN(countId) from #result)
set @maxid = (select max(countId) from #result)
 
while @maxid >= @minid
begin
              declare @itemid int
              declare @detailCollectionId int
 
              set @itemid = (select itemid from #result where countid = @maxId)
              print '@itemid: ' + cast(@itemid as varchar(50))
             
              truncate table #detailCollectionIds     
 
              --GET All collectionItemIDs
              insert into #detailCollectionIds
              select distinct detailCollectionId from n2detail where ItemID = @itemid and detailCollectionId is not null
             print 'ITEMID EFTER KONSTIGT SELECT: ' + cast(@itemid as varchar(50))
                    
              set @minDetailCollectionId = (select MIN(countid) from #detailCollectionIds)
              set @maxDetailCollectionId = (select Max(countid) from #detailCollectionIds)
 
              while @minDetailCollectionId <= @maxDetailCollectionId
              begin
                           --get collectionItemid for deleting in n2Item
                           set  @detailCollectionItemId = (select itemID from [n2DetailCollection] where ID = (select detailCollectionId from #detailCollectionIds where Countid = @minDetailCollectionId))
                           set @RealdetailCollectionId =(select detailCollectionId from #detailCollectionIds where Countid = @minDetailCollectionId)
                           print '@RealdetailCollectionId: ' + cast(@RealdetailCollectionId as varchar(50))
						   
						   

						   print '---------------------------------------------------------------------------'
						   print '@detailCollectionItemId: ' + cast(@detailCollectionItemId as varchar(50))
						   print '---------------------------------------------------------------------------'
                          
						
                           --delete item history, version control
                           --select * from n2Item where id = @detailCollectionItemId
						   if @detailCollectionItemId is not null 
						   begin
								if exists(select Countid from #result where itemid = @detailCollectionItemId) begin
									delete from n2Detail where itemid = @detailCollectionItemId                       
									insert into #log ([statement],itemid) VALUES ('1 delete from n2Detail where itemid =' + cast(@detailCollectionItemId as varchar(50)),@itemid)
								end
						   end

                           delete from n2Detail where detailCollectionId = @RealdetailCollectionId
                           insert into #log ([statement],itemid) VALUES ('2 delete from n2Detail where detailCollectionId =' + cast(@RealdetailCollectionId as varchar(50)),@itemid)
                          
                           delete from [n2DetailCollection] where ID = @RealdetailCollectionId
                           insert into #log ([statement],itemid) VALUES ('3 delete from [n2DetailCollection] where ID = ' + cast(@RealdetailCollectionId as varchar(50)),@itemid)
                           
						   --Some versioning
						   if exists(select Countid from #result where itemid = @detailCollectionItemId) begin
								delete from n2Item where ID = @detailCollectionItemId 
								insert into #log ([statement],itemid) VALUES ('4 delete from n2Item where ID =' + cast(@detailCollectionItemId as varchar(50)),@itemid)
							end                                                      

                           print '@@detailCollectionItemId: ' + cast(@detailCollectionItemId as varchar(50))                                                      
                           print '@minDetailCollectionId: ' + cast(@minDetailCollectionId as varchar(50))
                    
                           set @minDetailCollectionId = @minDetailCollectionId +1
              end
               
 
              --delete all the items on this item
              --select * from n2Detail where itemid = @itemid
			  if exists(select id from n2Detail where id = @itemid and linkvalue is not null)
			  begin					
					delete from n2Detail where itemid = (
									select distinct linkvalue 
									from n2Detail 
									where id = @itemid and linkvalue is not null)
									and linkvalue in(select itemid from #result)
						
					insert into #log ([statement],itemid) VALUES ('LINKVALUE delete from n2Detail where itemid = (
									select distinct linkvalue 
									from n2Detail 
									where id = ' + cast(@itemid as varchar(50)) +' and linkvalue is not null)
									and linkvalue in(select itemid from #result)',@itemid)
			  end


              delete from n2Detail where itemid = @itemid
              insert into #log ([statement],itemid) VALUES ('5 delete from n2Detail where itemid = ' + cast(@itemid as varchar(50)),@itemid)
 
              declare @n2DetailCollectionId int
              set @n2DetailCollectionId = (select id from dbo.n2DetailCollection where itemid = @itemid)
 
              delete from n2Detail where detailcollectionid = @n2DetailCollectionId
              insert into #log ([statement],itemid) VALUES ('6 delete from n2Detail where detailcollectionid =' + isnull(cast(@n2DetailCollectionId as varchar(50)),'nullvalue'),@itemid)
             
              delete from dbo.n2DetailCollection where itemid = @itemid
              insert into #log ([statement],itemid) VALUES ('7 delete from dbo.n2DetailCollection where itemid =' + cast(@itemid as varchar(50)),@itemid)
              --DELETE THE ITEM ONE BY ONE
              --select * from n2Item where id = @itemid
 
              delete from n2detail where linkvalue = @itemid
              insert into #log ([statement],itemid) VALUES ('8 delete from n2detail where linkvalue = ' + cast(@itemid as varchar(50)),@itemid)
 
 
              delete from n2AllowedRole where itemid = @itemid
              insert into #log ([statement],itemid) VALUES ('9 delete from n2AllowedRole where itemid = ' + cast(@itemid as varchar(50)),@itemid)
 
 
              delete from n2Item where id = @itemid
              insert into #log ([statement],itemid) VALUES ('10 delete from n2Item where id = ' + cast(@itemid as varchar(50)),@itemid)
 
 
 
              set @maxId = @maxId -1
end
 
--select * from #detailCollectionIds
 
--rollback tran
--commit tran
--12684
--select * from #log where id = 51305
--select * from #log where id between 51290 and 51315
--
--select * from n2Detail where id = 14158
--select * from #result where itemid = 2452
--select * from #result where itemid = 14865
--print 'delete from n2Detail where detailCollectionId =' + cast(@RealdetailCollectionId as varchar(50))

--select n1.*,n2.itemid as collectionItemid from n2Detail n1 
--inner join n2Detailcollection n2 on n2.id = n1.detailcollectionId
--where n1.stringvalue like '/wow-upplifun/hafu-samband/customer-service%'

--select * from n2DetailCollection where itemid = 14158

--select * from #detailCollectionIds

--SELECT * from n2Detail where itemid = (
--									select distinct linkvalue 
--									from n2Detail 
--									where id =67546and linkvalue is not null)
--									and linkvalue in(select itemid from #result)
```