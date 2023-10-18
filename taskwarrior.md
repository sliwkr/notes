# Taskwarrior

## Add a new task

```sh
task add Do this thing
```

## List tasks

- sorted by decreasing urgency

```sh
task  # same as 'task next'
task all  # show all tasks, including deleted, done
task all project:Dom  # list all tasks belonging to a given project
task completed  # list done tasks
task 1 export  # get ID 1 task as a json
task end.after:today completed  # list tasks completed since yesterday
```

- completed within a given time range

```sh
task end.after:2023-06-01 end.before:2023-07-01 completed
```

## Completing tasks

```sh
task 1 done  # mark task as completed
```

## Deleting tasks

```sh
task 1 delete  # remove task from 'task next', by changing its status
```

## Basic anatomy of a task

### Description
### Status
### Tags
    - Arbitrary words associated with a task
    - Use + to add a tag and - to remove a tag:
       - task add +home +work Water the flowers
       - task mod 3 +home
- Project
    - Each task can be associated with at least one project
      - $ task add project:Home Water the plants
    - Project can be divided into subproject with a dot .
      - $ task add project:Home.Kitchen Clean the sink
- Priority
    - $ task add pri:H Water the dying flowers
      - By default, only H,M,L values are supported
- Annotations
    - Associate more metadata with your task
    - Any number of annotations can be added to a task
    - $ task add Buy the milk
    - $ task 1 annotate Go to the shop on the corner
- Dates (due, until, scheduled date)
    - due date - if task is pending after due date, the task is considered overdue
    - scheduled date - eariest date i can work on the task
    - wait date - task disappears from the task list until that date
    - until date - if the task is not done up to that date, its deleted
    - due:year/month/date
    - due:ISO timestamp
    - due:{tomorrow,friday,yesterday,today,now}
    - due:{3wks,1day, 23rd, 9hrs}
    - due:{sow,eow}  # start of the next week, end of the week
    - due:soww  # start of the next work week
    - due:socw  # start of the current week
    - due:som  # start of the current month
    - due:eom  # end of the current month
    - due:soq  # start of the next quarter
    - due:soy  # start of the next year
    - due:goodfriday
    - due:easter
    - due:today+18h  # tomorrow in the middle of the day

## Edit a task

- open editor for metdata of particular task, creates annotation with the date pointing to the moment of executing this command

```sh
task 2 edit
```

- just add a note (annotation)

```sh
task 1 annotate Just typing a note
```

- add a due/scheduled/wait/until date
```sh
task 1 mod due:eom
```

## Show details about a task

```sh
task 2 info
task 2  # same as task 2 info
```

## Undo an action on a task

```sh
task undo
```

## Completely, for sure, remove a task forever

- a task has to be already deleted
- UUID is used to identify a task to delete
```sh
task all project:Party
task 51ad4d3c delete
task 51ad4d3c purge
```


## Task identifiers

### ID
- Line number in pending.data file

### UUID
- Unique permanent reference to the task, never changes


## Urgency

## Filters & Reports

- filter - query to subselect a set of tasks
- report - next, all, list
- e.g. `task completed` report has a default filter which filters completed tasks

```sh
task due.before:eom priority.not:L list  # list tasks due til end of the month that are not low priority
# other attribute modifiers:
before (synonyms under, below)
after (synonyms over, above)
none
any
is (synonym equals)
isnt (synonym not)
hasnt
startswith (synonym left)
endswith (synonym right)
word
noword
```


### using filter expressions

- `=` operator checks just for the day - hours and minutes are ignored
- `==` operator checks for exact equality

```sh
and, or , xor, !  # logical operators
< <= = == != !== >= >  # relational operators
(  )  # precedence

task due.before:eom priority.not:L list
task '( due < eom or priority != L )' list  # same as the line above
task '! ( project:Home or project:Garden )' list  # get all that are not project home or garden
```

### reports

- each report has a default filter
- by default, each report is affected by the current context
- current context can be ignored by report.<name>.context=0

```sh
task show  # show configuration values
task show report.list.filter
```

## configuration

- ~/.taskrc file
- ~/.task/backlog.data  # important for syncing
- ~/.task/completed.data
- ~/.task/pending.data
- ~/.task/undo.data  # captures all changes, grows, fine to delete a bunch of stuff at some point here

## recurrence, repeating

See [Tags] for adjusting recurrence times

- create recurrence template from which further tasks will be generated
```sh
task add Take out the trash due:friday recur:weekly  # creates a template and, if parameters allow for that, a task
## Created task 7 (recurrence template)
task 7 8 mod wait:eoww  # modify recurrence template and the current task to wait til end of the work week
task add Kalistenika project:dzik due:tomorrow recur:2d  # create a task that has a due date and occurs every second day
```

## timetracking


```sh
task 3 start
task 3 info
task 3 stop

```


## virtual tags

- autogenerated task based on task creation parameters, not visible in tasks file

```sh
task due:today
task info 1  # each task has Virtual tags row
task +TODAY  # this is a virtual tag
task +ANNOTATED  # also a virtual tag
task +LATEST  # useful virtual tag
```

## taskserver

- means of syncing changes between changes
- communicates over TCP w/ TLS
- taskserver has a competing project, taskd
- not possible to setup with just ssh
- rtfm
- latest modification means valid version:
    - naive: OS time differences will affect which task will be the valid one(guess)

```sh
task sync
```

## scheduling & waiting, due date, until date

- When a task is `scheduled`, it appears on `task` and `task ready` lists. The date marks the earliest opportunity to work on it,
- When a task is `wait`ing, it is hidden on the above lists until the date comes. Can still be viewed by `task waiting`
- When a task has a `due` date, it's self-explanatory,
- When it has an `until` date, it is removed from the list after the date should i not complete it

```sh
task add Zakupy wait:tomorrow due:tomorrow+1d  # add a task which will appear on the list and will have to be done tomorrow
task project:X +READY mod wait:+30d  # select project X, delay all current tasks by 30d
```

## context

- way of avoiding constatntly setting filters
- specify current work context by automatically filtering irrelevant tasks
- specify context within .taskrc
```sh
context.krzysztof=project:CzytajPlz
context.home=project:Personal
```
- use the specified context
```sh
task @ home  # no tasks from projects other than Personal shown
task context none  # unset whichever context was set
task context define newContextName 'project:smProject +TAG_FILTER'  # create new context
```

## UDA - user defined attributes

- default user defined attributes
- way of overwriting values for existing attributes
```sh
task show uda
```

## Taskwarrior DOM

- no need to grep the TUI

```sh
task _get 1.description  # return description from task with ID 1
```

## Hooks

- allow to perform custom action each time a task is added
- in ~/.task/hooks
- needs to have a specific name, e.g. do-this.sh.on-modify will work on modifications
- it'll get on input, a json format of tasks fitting the action:
    - e.g. *.on-modify will get a json of a task being modified
- e.g. modify due date from 00:00 to another hour further in the day
- TODO is this just a script or smth executed within taskwarrior context

## tasklib

- python library to access your tasks
- potentially useful for hooks

## specifying dependencies

```sh
task 49 mod deps:48  # modify 49 as being blocked by 48
task +BLOCKING  # show all blocking tasks
task +BLOCKED  # show all blocked tasks
task +BLOCKING -BLOCKED  # show blocking tasks which are not blocked by others
```

## taskwiki

- combination of vimwiki and taskwarrior
- way to define a project, list of tasks by creating a single note, that defines stuff to be done
```sh - index.wiki - taskwiki way of creating a new task
==== Tasks ====
* [ ] Get the cake
```

## worthwhile plugins
- bugwarrior
- taskopen

## consider contributing

