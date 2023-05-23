
```sh
timew help continue  # display a manpage for 'continue' command

timew start  # start an entry, add the new entry to summary
timew start 'Using Tags' Software  # start an entry and assign 2 tags to it
```


```sh
timew stop  # stop currently running entry
timew continue  # resume the previous entry
```

```sh
timew summary
timew summary Software  # summary for entries with Software tag
timew {day,week,summary} TagName <range>  # show entries with a given tag within given time ranges
timew summary Programming sow - now
timew summary :ids  # show ID column
timew summary 2022-08-16  # show entries from a given date
timew summary som - now  # summary for the current month
timew summary :week :ids  # show entries for the current week with @ID
timew delete @ID  # delete an entry
```

```sh
timew tags  # created tags
timew config tags.Software.description 'Learning about new software'  # add a description to a tag
```

```sh
timew track 10:30 - 11:00 'Daily'  # add an entry in the past
timew track 10:30 - 11:00 'Daily' :adjust  # insert the activity in the middle of an existing entry
timew help ranges  # more accepted time ranges 
timew help dates  # more accepted date formats
timew track 8am for 1h 'Coffee'
```


```sh
timew help hints  # show manpage about :hints
timew track 9am - 10am 'Walk the dog' :quiet  # disable verbosity with :quiet hint
```


```sh
# charts
timew day 
timew day sow - now
timew week
timew month
```


```sh
timew gaps  # show untracked time
```


```timewarrior.cfg
# modify color theme
import /usr/local/share/doc/timew/themes/dark_green.theme
```

```timewarrior.cfg
# mark holidays
import /usr/local/share/path/to/holidays/holidays.pl-PL
```

```sh
timew config exclusions.days.2022_08_21 off :yes  # exclude a given day from tracking
timew config exclusions.monday '<8:30 12:30-13:15' :yes  # each monday, exclude part of the day before 8:30 and a break during the day
timew week :blank  # view just the exclusions
```


```sh
# correcting entries:
timew summary :ids  # :ids allow to identify a single entry
timew move @3 8:30am  # move the whole entry to start at 8:30am (modifies start & end time, does not modify duration)
timew lengthen @3 30mins  # lengthen the duration of an event by 30mins (modifies end time and duration, does not modify start time)
timew move @3 10:02 :fill  # given an entry which ends on 10:00, an entry with id==3 and an entry which starts at 11:00, modify @3 so it starts at 10:00 and ends at 11:00
timew split @3  # split the interval in 2 equal parts
timew untag @4 @3 ProjectB  # remove tag from entries
timew tag @4 ProjectB1  # retag
timew tag @3 ProjectB2
timew join @2 @1  # join 2 entries together
```


```sh
timew extensions  # extensions
timew diagnostics  # debug info, extensions aswell
```


# bugs:

```sh
# 1.4.2 an entry has to be stopped beforehand
$ timew start 12pm ProjectC
$ timew summary :ids  # grab the id
$ timew move @1 11:00
Internal error. Failed encode / decode check.
```


```sh
# 1.4.2 exclusions on does not take effect
$ timew config exclusions.days.2022_08_21 off :yes
$ timew week  # shows week with the given day excluded
$ timew config exclusions.days.2022_08_21 on :yes
$ timew week  # no changes on the chart
# remove exclusion line from timewarrior.cfg
$ timew week  # exclusion not visible on the chart
```
