ATTRIBFT.LLL v0.7.1 (20220713), by deomsh (subroutines :checksumSFN and :help originated by Wonko the Sane)
Functions: Find FAT12/ FAT16/ FAT32 file/ directory attributes, volume special case Further: get/ set directory entries related to Short and Long File Names
Use: ATTRIBFT.LLL <function> [Optional Argument(s)] <FILE> [<parameter(s)>]
Use: ATTRIBFT.LLL (without arguments) shows all function-names
Use: ATTRIBFT.LLL <help> [<function-name>] <help function> gives specific help
Mandatory Arguments: function FILE and function-specific parameter(s)
Remark Mandatory Arguments: functionname not case sensitive, most functions too. Without device/ path on current ROOT
Remark Long File Names: between double-quotes or use escaped spaces: '\ '. Double qoutes mandatory if using '='
Remark Volume label: if searched give 8+3 filename. Uppercase supported only
Remark Parameters: function-specific, AND after FILE
Remark Returns: ONLY if 'success-variable' 'result' exists, content is function-specific. If any error: variable 'message' exists, containing textual error. Variable 'output' consists of set-variables, device-specific and mostly FAT-bootsector-specs
Remark Optional Arguments: order is NOT free, MUST given before FILE
1 <mdbase=N-sectors> (no qoutes!) changes base-address of working memory (default 0x20000, range 0x220:0x3FFFF). No check for ranges used by Grub4dos!
2 <input="%output%"> (only mandatory double-qoutes for second part!) for inserting predefined set-variables (with && ) to speed up operation

List of Functions
Attribute: getattrib; setattrib
Date and Time: getdate; gettime; getaccdate; getcreadate; getcreatime; getdatetimeall; getdatetimeattriball; setdate; settime; setaccdate; setcreadate; setcreatime; setdatetime; setcreadatetime
Service: attribecho; attriballecho; datetimeecho; datetimeallecho; datetimeattribecho
Long File Names: getsfn; getsfnpath; getsfnfile; getlfn; makelfn; dellfn
Special: isdir; isreadonly; getlabel; setlabelattrib; getntcasebyte; geteapointer; calcsfnchecksum; calclfnhash; getlastclusnum; getchainclus
Internals: getsfnentry; getlfnentry; getlfnentries; getfreeentry; getfreeentries; getdelentry; getdelsfnentry; getlabelentry

Grub4Dos only, should work on version 0.46a 2017-05-05 and upwards (Grub4Efi untested)

Simple debug echo-system still active: give 'echo' (without qoutes) as last argument to 'view' what is going on (in most funtions)

ATTRIBFT.G4B is a simple Grub4Dos command-line frontend for ATTRIBFT.LLL, will sent arguments/ parameters to ATTRIBFT.LLL - must reside in same directory

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
