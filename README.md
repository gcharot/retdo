# RETDO - The backup retention SwissKnife

retdo is a simple Shell script that allows administrators to clean up files on a custom retention basics. retdo can be used to implement production's backups retention plans.

# WHAT CAN I DO

retdo can resolve the following queries :

* I want to keep only one file per week if files are older than 3 months up to 6 months.
* I want to keep only one file per month if files are older than 6 months up to 1 year.
* I want files older than 1 year to be moved to another machine.
* I want a cup of tea (feature in progress)

# USAGE

You'll find the options list below, nevertheless always keep that in mind : 

* ALWAYS use the simulate option (-s), this option will tell you which files will be processed. When you are happy with the results, you can go ahead and run the script for real.

* It is STRONGLY recommended to use the regexp option (-r), by default retdo process all files (*) of a given directory as they were the same. If you have backup files (.tgz or .gpg) and MD5 files, you need to run retdo two times, one for the backup files and one for the MD5 files. Again use -s beforehand to check the results.

* Default action is to DELETE files, if you tell retdo to keep 1 file per week then 1 file per week will be kept and the other will be deleted. If you want to copy, move, scp, etc, use the -a option.

```
Usage : retdo OPTIONS
        Cleanup files with custom retention periods.

        -a, --action "cmd MATCH_FILE"   Execute custom action (optional : Use "rm -f MATCH_FILE" by default).
        -b, --begin N                   Cleanup files older than N DAYS (required)
        -d, --delta N                   Keep one file every N DAYS (required). delta=1 means delete all files.
        -e, --end N                     Cleanup files younger than N DAYS (required)
        -h, --help                      Display this help and exit
        -p, --path PATH                 Path to files (required). retdo delete only files NOT directories
        -r, --regexp "REGEXP"           Files mathing REGEXP only (optional : Use * by default)
        -s, --simulate                  Don't make any action, just simulate (optional, implies -v)
        -v, --verbose                   Print debug information (optional)


Actions :
Default action is to delete files matching the specified retention values. For example if you tell retdo to keep 1 file per week then 1 file per week will be kept and the other will be deleted.
You can configure retdo to make any other action you like. However you will need to enter the full command like you would normally do in a shell; just replace the filename part with the MATCH_FILE expression.

Exemples :
* Move files to /data/backups/old/ :
        -a "mv MATCH_FILE /data/backups/old/"
* Copy files to a remote server
        -a "scp MATCH_FILE user@remote:/data/backups/old/"

Restrictions :
* Your filenames/path must NOT containt the string MATCH_FILE.
* Your filenames/path must NOT containt the '#' character
* Always check your custom commands with the simulation feature (-s)


retdo Usage examples :

* Parse all files named *.tgz in /data/backups/DB that are older than 3 months (90 days) and younger than 6 months(181 days) then keep only one file per week (7days) :
        $ retdo -p /data/backups/DB -r "*.tgz" -b 90 -e 181 -d 7

* Same thing but don't do any action just show me what you would have done (simulate) :
        $ retdo -p /data/backups/DB -r "*.tgz" -b 90 -e 181 -d 7 -s

* Delete all files named *.tgz in /data/backups/DB that are older than 3 months (182 days) and younger than 1 year (365 days) :
        $ retdo -p /data/backups/DB -r "*.tgz" -b 182 -e 365 -d 1

* Parse all files in /data/backups/DB that are older than 6 months (182 days) and younger than 1 year(364 days) then keep one file per month (30 days) and move the others to /data/backups/DB/old/ :
        $ retdo -p /data/backups/DB -b 182 -e 364 -d 7 -a "mv MATCH_FILE /data/backups/DB/old/"

```
