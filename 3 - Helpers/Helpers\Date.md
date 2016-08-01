The Date helper is used for calculations with dates.

````php
Date::difference($from, $to, $type = null)
```

* **$from, $to** - from and to date in the format: YYYY-MM-DD HH:MM::SS

* **$type** - default to null will return an array containing:

````php
[y] => 0
[m] => 2
[d] => 26
[h] => 0
[i] => 0
[s] => 0
[weekday] => 0
[weekday_behavior] => 0
[first_last_day_of] => 0
[invert] => 1
[days] => 86
[special_type] => 0
[special_amount] => 0
[have_weekday_relative] => 0
[have_special_relative] => 0
```

to return a set item like the number of days pass d as the third parameter.

````php
Date::businessDays($startDate, $endDate, $weekendDays = false)
```

Get the number of days between 2 dates, set $weekendDays to true to get the number of weekends.

````php
Date::businessDates($startDate, $endDate, $nonWork = 6)
```

Get an array of dates between 2 dates (not including weekends). $nonWork day of week(int) where weekend begins - 5 = fri -> sun, 6 = sat -> sun, 7 = sunday

````php
Date::daysInMonth($month = 0, $year = '')
```

Takes a month/year as input and returns the number of days for the given month/year. Takes leap years into consideration.