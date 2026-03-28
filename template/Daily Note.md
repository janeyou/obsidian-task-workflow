# {{date}}

## Focus
-

## Due Today
```tasks
not done
due on {{date}}
status.name does not include Pending
sort by priority
```

## Pending (waiting on others)
```tasks
status.name includes Pending
sort by priority
group by folder
```

## Overdue
```tasks
not done
due before {{date}}
sort by priority
group by folder
```

## Quick Capture
-

## Notes


## Completed Today
```tasks
done
done on {{date}}
sort by done reverse
group by folder
```

## Tomorrow
```tasks
not done
due on tomorrow
sort by priority
group by folder
```

## This Week
```tasks
not done
filter by function !!(task.due.moment?.isAfter(moment().add(1, 'day'), 'day') && task.due.moment?.isSameOrBefore(moment().endOf('isoWeek'), 'day'))
sort by priority
group by folder
```

## Next Week
```tasks
not done
filter by function !!(task.due.moment?.isAfter(moment().endOf('isoWeek'), 'day') && task.due.moment?.isSameOrBefore(moment().add(1, 'week').endOf('isoWeek'), 'day'))
sort by priority
group by folder
```

## Future
```tasks
not done
filter by function !!task.due.moment?.isAfter(moment().add(1, 'week').endOf('isoWeek'), 'day')
sort by due
group by folder
```

## No Due Date
```tasks
not done
no due date
sort by path
group by folder
```
