## ATTRIBFT.LLL v0.9.2 (20250827), by deomsh
<pre><code>Functions: Find FAT12/ FAT16/ FAT32 file/ directory attributes, volume special case
Further: get/ set directory entries related to Short and Long File Names
Use: ATTRIBFT.LLL function [Optional Argument(s)] FILE [parameter(s)]
Use: ATTRIBFT.LLL (without arguments) shows all function-names
Use: ATTRIBFT.LLL help [function-name] gives specific help on function
Mandatory Arguments: function FILE and function-specific parameter(s)
Remark Mandatory Arguments: functionname not case sensitive, most functions too. Without device/ path on current ROOT
Remark Long File Names: between double-quotes or use escaped spaces: '\ '. (or '\=' if using '=')
Remark Volume label: if searched give 8+3 filename. Uppercase supported only
Remark Parameters: function-specific, AND after FILE
Remark Returns: ONLY if 'success-variable' 'result' exists, content is function-specific. 
If any error: variable 'message' exists, containing textual error. 
Variable 'output' consists of set-variables, device-specific and mostly FAT-bootsector-specs
Remark Optional Arguments: order is NOT free, MUST given before FILE
1 mdbase=N-sectors (no qoutes!) changes base-address of working memory (default 0x20000, range 0x220:0x3FFFF)
  No check for ranges used by Grub4dos!
2 input="%output%" (only mandatory double-qoutes for second part!) for inserting predefined set-variables (with && ) to speed up operation

List of Functions
Attributes: getattrib; setattrib; setlabelattrib
Date and Time: getdate; gettime; getaccdate; getcreadate; getcreatime; getdatetimeall; getdatetimeattriball; setdate; settime; setaccdate; setcreadate; setcreatime; setdatetime; setcreadatetime
Service: attribecho; attriballecho; datetimeecho; datetimeallecho; datetimeattribecho
Long File Names: getsfn; getsfnpath; getsfnfile; getlfn; makelfn; dellfn; getlfnpath
Special: isdir; isreadonly; getlabel; getntcasebyte; setntcasebyte; geteapointer; calcsfnchecksum; calclfnhash; getlastclusnum; getchainclus
Internals: getsfnentry; getlfnentry; getlfnentries; getfreeentry; getfreeentries; getdelentry; getdelsfnentry; getlabelentry

Grub4Dos only, should work on version 0.46a 2017-05-05 and upwards (NO check!)
Grub4dos for UEFI is tested, boot rather soon 'out of malloc memory' (last version 20240901)

Simple debug echo-system still active: give 'echo' (without qoutes) as last argument to 'view' what is going on (in most funtions)

(subroutines :checksumSFN and :help originated by Wonko the Sane)</code></pre>    

### HISTORY  
Changes V0.9.2  
1) BUGFIX: not compatible with partitions >= ~256GB  
2) BUGFIX: function getsfn produced space after extension if length 1 or 2 chars  

Changes V0.9.1  
1) BUGFIX: for use with 'help' unload ATTRIBFT.LLL if still loaded with insmod  
2) BUGFIX: bettter compatibility if full grub4dos script-line is part of 'name' or 'path' or read-out from disk  

Changes V0.9  
1) New: function 'getlfnpath'  
2) New: label-functions returns message if no label is found  
3) New: compatible with only ONE File Allocation Table (number of FAT's = 1)  
4) BUGFIX: functions 'isdir' and 'isreadonly' not compatible if on disk >= 4GB (should now be working < 1TB)  
5) BUGFIX: functions 'settime' and 'setattrib' and 'makelfn' sfnentry not found in certain cases if namepart < 8 chars  
6) BUGFIX: sfnchecksum '%' detected if '25' in filename  
7) BUGFIX: in sub-routine ':issfnattrib'  
8) BUGFIX: regarding debug switch 'echo'  

Changes V0.8  
1) Improroved FAT-detection: number of root-entries checked for multiples of 16  
2) Some changes to content of message-system  
3) Help-system cleaned and with better outline (now about 60 examples)  
4) Variable 'result' for functions 'getsfn'. 'getsfnpath'. 'getsfnfile' now always uppercase  
5) Output formats changed, OR variable 'result' OR variable 'message' (now 'result=1' if 'value' already existed and not had to be set)  
6) After use 'debug msg=0' (no longer to ' debug msg=3')  
7) More value-formats accepted in label-functions  
8) Change of default 'mdbase' to 0x1FE00 [max 256k to 0x20000]  
9) New function: 'setntcasebyte' (to 0x0, 0,8, 0x10 or 0x18)  
10) Char '=' allowed in PATH  
11) Bugfix ignoring not existing directories in PATH if remaining PATH valid  
12) Read-out bytes on disk above 4GB  
13) Bugfix in parsing output of 'vol' if volumename contains wrong file system abbreviation  
14) Bugfix in function 'makelfn' if Long File Name already exist  

Changes V0.7.1.0.1:  
Out-remarked cat --hex at line 1108  

Changes V0.7.1:  
1) Bugfix in function 'getlfn' for Grub4dos operators ' && ', ' &; ', ' ;; ' and ' ! '  
2) Bugfix for sub-routine ':lastcluster' if 1, 2 or 3 free clusters on FAT12 or 1 free cluster on FAT16/ FAT32: no addition of (emptied!) directory cluster to the directory-clusterchain  
3) Bugfix for sub-routine ':lastcluster' if directory-clusterchain is already 2MB, no addition of (emptied!) directory cluster to the directory-clusterchain  
4) More protection of command-line if arguments contains '%' (except %u, %s, %x and %X => these must be taken care off 'by Software')  
5) Grub4dos color-codes (starting with '$[0x') added to list of forbidden chars (function 'getlfn' not affected, Software should take care if echoing!)  
6) Grub4dos operators ' && ', ' &; ', ' ;; ' and ' ! ' can be also used in Long File Names in PATH (full FILE between double-qoutes mandatory: "FILE" OR use escaped '\ ' spaces)  

Changes V0.7.0:  
1) Bugfix in function 'getsfn' in case Long File Name < 11 chars (fix needed in 'newer' Grub4dos versions)  
2) Bugfix in function 'getsfn' in case Long File Name in path, but file is Short File Name  
3) Bugfix in function 'getlfn' for root (fat12/ FAT16)  
4) Bugfix for function 'getchainclus'  
5) Function 'dellfn' will search full cluster-chain in a directory and delete ALL entries of searched Long File Name (Short File Name entry still untouched!)  
6) Function 'setlabelattrib': label now new argument after DEVICE, instead DEVICE/LABEL  
7) Starting spaces allowed in Long File Name in all functions of relevance, EXCEPT function 'makelfn'  
8) Some protection of command-line, most functions will abort if too many arguments are used  
9) Grub4dos operators ' && ', ' &; ', ' ;; ' and ' ! ' can be used in Long File Names (full FILE between double-qoutes mandatory: "FILE")  

Changes V0.6:  
1) bugfix for searching Short File Names if second name is a subset of first name and last byte of ENTRY is same as attribute.  
2) faster execution of function 'getlfn'  
3) new function 'setlabelattrib'. For specs use: ATTRIBFT.LLL help setlabelattrib  
4) dropped function 'getnumclus', is contained in set-variable %output% anyway  


## ATTRIBFT.G4B v.0.4 (20241109), by deomsh
ATTRIBFT.G4B is a simple Grub4Dos command-line frontend for ATTRIBFT.LLL  
will sent arguments/ parameters to ATTRIBFT.LLL - must reside in same directory  

### HISTORY  
Changes V0.4:  
NEW: check if grub4dos version >= 20270505 is in use  

Changes V0.3:  
Bugfixes  
NEW: function 'viewentry' = 'cat --hex' ready to run, + FULL readout [%output% gives '%checkdev%' !]  
NEW: function 'viewdir' = 'cat --hex' ready to run, + FULL readout [%output% gives '%checkdev%' !]  

Changes V0.2:  
Highcol output for result if starting spaces in Long File Name  

### SCREENSHOTS
![TEXTSTAT G4B ATTRIBFT LLL version 0 9 2 after cleaning](https://github.com/user-attachments/assets/362c8dcb-d856-4df2-9d36-68251195aa0b)
