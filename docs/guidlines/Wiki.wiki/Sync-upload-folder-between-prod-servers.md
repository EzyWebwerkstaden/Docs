Guide:

- Install "FreeFileSync": get its payed version without ads from \\192.168.20.100\ProgramFiler\FreeFileSync_7.8_Windows_Setup_(for_Donors)
- In "FreeFileSync" choose what folders to sync and then save task as a batch file.
- Share the upload-folder on second server so that first server (that runs FreeFileSync) has access to that folder.
- Create new task in "Task Scheduler" to run this batch file every 5 minutes (Task Scheduler is a standard Windows app)

Läs mer: https://jira.ezy.se/browse/JETS-93