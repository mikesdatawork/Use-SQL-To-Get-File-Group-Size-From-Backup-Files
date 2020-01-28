![MIKES DATA WORK GIT REPO](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_01.png "Mikes Data Work")        

# Use SQL To Get File Group Size From Backup Files
**Post Date: June 3, 2014**        



## Contents    
- [About Process](##About-Process)  
- [SQL Logic](#SQL-Logic)  
- [Build Info](#Build-Info)  
- [Author](#Author)  
- [License](#License)       

## About-Process

<p>Here's a quick script to get the Data File Size using just the backup file. Basically; this is good to do before you run a Database restore so you can see where you need to place the files on your drives. Even though the restore will tell you if you are low on space ( as an error ). Additionally; if you're like me you'll probably want to write out all the logic for the restore. The results of this script will help you see all the data file lists so your code is consistent.</p> 


## SQL-Logic
```SQL
use master;
set nocount on
 
declare @filelist_from_backup_file table
(
LogicalName nvarchar(128),
PhysicalName nvarchar(260),
[Type] char(1),
FileGroupName nvarchar(128),
Size numeric(20,0),
MaxSize numeric(20,0),
FileID bigint,
CreateLSN numeric(25,0),
DropLSN numeric(25,0),
UniqueID uniqueidentifier,
ReadOnlyLSN numeric(25,0),
ReadWriteLSN numeric(25,0),
BackupSizeInBytes bigint,
SourceBlockSize int,
FileGroupID int,
LogGroupGUID uniqueidentifier,
DifferentialBaseLSN numeric(25,0),
DifferentialBaseGUID uniqueidentifier,
IsReadOnl bit,
IsPresent bit,
TDEThumbprint varbinary(32) -- remove this column if using SQL 2005 )
insert into @filelist_from_backup_file
exec('restore filelistonly from disk = ''\\MyBackupShare\MyBackupFile.bak''')
 
select
fileid
, logicalname
, 'size in gb' = ( convert(decimal(10,2),size / 1024 / 1024 / 1024))
, 'bu size in gb' = ( convert(decimal(10,2),backupsizeinbytes / 1024 / 1024 / 1024 )) , 'physical path' = physicalname
from
@filelist_from_backup_file
order by
size desc
```


[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

[![Gist](https://img.shields.io/badge/Gist-MikesDataWork-<COLOR>.svg)](https://gist.github.com/mikesdatawork)
[![Twitter](https://img.shields.io/badge/Twitter-MikesDataWork-<COLOR>.svg)](https://twitter.com/mikesdatawork)
[![Wordpress](https://img.shields.io/badge/Wordpress-MikesDataWork-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

   
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Mikes Data Work](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_02.png "Mikes Data Work")

