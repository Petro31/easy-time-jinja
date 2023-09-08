[![hacs_badge](https://img.shields.io/badge/HACS-Default-orange.svg)](https://github.com/custom-components/hacs)
![Version](https://img.shields.io/github/v/release/Petro31/easy-time-jinja)

<a href="https://www.buymeacoffee.com/Petro31"><img src="https://img.buymeacoffee.com/button-api/?text=Buy me a coffee&emoji=&slug=Petro31&button_colour=5F7FFF&font_colour=ffffff&font_family=Poppins&outline_colour=000000&coffee_colour=FFDD00" width=auto, height=30/></a>


# Easy Time

Tired of getting help performing math with time?  Look no further.  This Template extension for home assistant makes time easy calculations easy!

# Installation

Install this in HACS or download the `easy_time.jinja` from this repository and place the files into your `config\custom_templates` directory.

After installation, you can edit the first line to set a default language, this will make the macros easier to use in your native language.

```
{%- set default_language = 'en' %}
```

# Languages

The current supported languages are:

* English
* Dutch - Thanks [TheFes](https://github.com/TheFes)
* Swedish - Thanks [Hellis81](https://community.home-assistant.io/u/hellis81/summary)
* German - Thanks [kCologne](https://community.home-assistant.io/u/kCologne)
* Danish - Thanks [CMDK](https://community.home-assistant.io/u/cmdk/summary)
* French - Thanks [baylanger](https://github.com/baylanger)
* Spanish - Thanks [Didgeridrew](https://community.home-assistant.io/u/Didgeridrew)
* Italian - Thanks [Gianpi](https://github.com/jumping2000)
* Portuguese - Thanks [FragMenthor](https://github.com/FragMenthor)
* Croatian - Thanks [IgorSimic](https://github.com/IgorSimic)
* Polish - Thanks [Szymon Jankowski](https://github.com/sjankowski)
* Norwegian (Bokmål and Nynorsk) - Thanks [Peder Stray](https://github.com/pstray/)

# Time Macros

## `clock(time_format)`

A simple clock's time.  Using `clock()` without arguments will use your languages settings.

Argument | Type | Default | Example | Description
:-:|:-:|:-:|:-:|---
time_format| str | - | `'12-hr'` | (Optional) `'12-hr'`, `'24-hr'`

Format | Output
:-:|:-:
`'12-hr'` | 8:45 AM
`'24-hr'` | 08:45

### Examples

```jinja
{% from 'easy_time.jinja' import clock %}
{{ clock() }}
```

## `clock_icon(hour)`

A simple clock's icon.  Using `clock_icon()` without arguments will produce the current time's clock icon to the nearest half hour.

Argument | Type | Default | Example | Description
:-:|:-:|:-:|:-:|---
hour| int | - | `1` | (Optional) Hour of the day.

### Examples

```jinja
{% from 'easy_time.jinja' import clock_icon %}

{# Return the current time icon #}
{{ clock_icon() }}

{# Return midnight or noon's current time icon #}
{{ clock_icon(0) }}
{{ clock_icon(12) }}
```

# Relative Time Macros

Looking for times in the future or the past in your language?  Look no further.  These easy macros will pave the way to the future...

Also, please check out [Relative-Time-Plus](https://github.com/TheFes/relative-time-plus) by TheFes.  It has differnt options for relative time, and it's more flexible than `easy_custom_time`.

## `easy_time(entity_id_or_time)`

`easy_time` returns the most significant friendly relative time.  For example, if your uptime is 3 hours, 2 minutes, and 1 second, this macro will return `3 hours` in your default language.

Argument | Type | Default | Example | Description
:-:|:-:|:-:|:-:|---
entity_id_or_time| string, datetime, or entity_id | - | `'sensor.uptime'` | (Required) The entity_id, date string, or datetime object.
attribute| str or None | No | `None` | (Optional) attribute to extract the desired time from.
language| string | set by user | `'en'` | (Optional) Override the default language.
utc| boolean | `False` | `True` | (Optional) If your `uptime` argument does not have a timezone and you wish to treat it as a UTC timestamp, set this to True.  Otherwise the function assumes `Local` calculations.
max_period| string | `'year'` | '`hour`'| (Optional) Truncate the maximum significant period.  Available options: `year`, `week`, `day`, `hour`, `minute` and `second`

### Examples

```jinja
{% from 'easy_time.jinja' import easy_time %}

{# From an entity state #}
{{ easy_time('input_datetime.alarm_time') }}

{# Last Updated #}
{{ easy_time(states.sensor.my_energy_meter.last_updated) }}

{# Calendars - start time #}
{# By default, easy_time macros assumes you'd like the start time #}
{{ easy_time('calendar.my_events') }}
{# Calendars - end time #}
{{ easy_time('calendar.my_events', 'end_time') }}

{# Overriding language or utc entity attribute #}
{{ easy_time("2023-04-07 00:00:00", None, 'en', True) }}
{{ easy_time("2023-04-07 00:00:00", language='en', utc=True) }}
{{ easy_time("2023-04-07 00:00:00", max_period='hour') }}
{{ easy_time('calendar.my_events', 'end_time', 'en', True) }}
{{ easy_time('calendar.my_events', 'end_time', language='en', utc=True) }}
```

## `big_time(entity_id_or_time)`

`big_time` returns the friendly relative time without missing any details.  For example, if your uptime is 3 hours, 2 minutes, and 1 second, this macro will return `3 hours, 2 minutes, and 1 second` in your default language.

Argument | Type | Default | Example | Description
:-:|:-:|:-:|:-:|---
entity_id_or_time| string, datetime, or entity_id | - | `'sensor.uptime'` | (Required) The entity_id, date string, or datetime object.
attribute| str or None | No | `None` | (Optional) attribute to extract the desired time from.
language| string | set by user | `'en'` | (Optional) Override the default language.
utc| boolean | `False` | `True` | (Optional) If your `uptime` argument does not have a timezone and you wish to treat it as a UTC timestamp, set this to True.  Otherwise the function assumes `Local` calculations.
max_period| string | `'year'` | '`hour`'| (Optional) Truncate the maximum significant period.  Available options: `year`, `week`, `day`, `hour`, `minute` and `second`

### Examples

```jinja
{% from 'easy_time.jinja' import big_time %}

{# From an entity state #}
{{ big_time('input_datetime.alarm_time') }}

{# Last Updated #}
{{ big_time(states.sensor.my_energy_meter.last_updated) }}

{# Calendars - start time #}
{# By default, easy_time macros assumes you'd like the start time #}
{{ big_time('calendar.my_events') }}
{# Calendars - end time #}
{{ big_time('calendar.my_events', 'end_time') }}

{# Overriding language or utc entity attribute #}
{{ big_time("2023-04-07 00:00:00", None, 'en', True) }}
{{ big_time("2023-04-07 00:00:00", language='en', utc=True) }}
{{ big_time("2023-04-07 00:00:00", max_period='hour') }}
{{ big_time('calendar.my_events', 'end_time', 'en', True) }}
{{ big_time('calendar.my_events', 'end_time', language='en', utc=True) }}
```

## `custom_time(entity_id_or_time, values)` and `custom_time_attr(entity_id, attribute, values)`

`custom_time` and `custom_time_attr` returns the friendly relative time providing details that match your needs.  For example, if your uptime is 3 hours, 2 minutes, and 1 second, this macro will return `3 hours and 2 minutes` in your default language.

Argument | Type | Default | Example | Description
:-:|:-:|:-:|:-:|---
uptime| string, datetime, or entity_id | - | `'sensor.uptime'` | (Required) The entity_id, date string, or datetime object.
attribute| str or None | No | `None` | (Required for `custom_time_attr`) attribute to extract the desired time from.
values | string | none | `'day, hour, minute'` | (Required) Options for displaying time.  Available options: `year`, `week`, `day`, `hour`, `minute` and `second`. 
language| string | set by user | `'en'` | (Optional) Override the default language.
utc| boolean | `False` | `True` | (Optional) If your `uptime` argument does not have a timezone and you wish to treat it as a UTC timestamp, set this to True.  Otherwise the function assumes `Local` calculations.

### Examples

```jinja
{% from 'easy_time.jinja' import custom_time, custom_time_attr %}

{# From an entity state #}
{# NOTE: the value after the = sign for minute and hour can be anything. #}
{{ custom_time('input_datetime.alarm_time', 'hour, minute') }}

{# Calendars - start time #}
{# By default, easy_time macros assumes you'd like the start time #}
{{ custom_time('calendar.my_events', 'hour, minute') }}
{# Calendars - end time #}
{{ custom_time_attr('calendar.my_events', 'end_time', 'hour, minute') }}

{# Last Updated #}
{{ custom_time(states.sensor.my_energy_meter.last_updated, 'hour, minute') }}

{# Overriding language or utc entity attribute #}
{{ custom_time("2023-04-07 00:00:00", 'hour, minute', language='en', utc=True) }}
{{ custom_time_attr('calendar.my_events', 'end_time', 'hour, minute', language='en', utc=True) }}
```

## `easy_relative_time(entity_id_or_time)`

`easy_relative_time` returns the most significant friendly relative time.  For example, if your uptime is 3 hours, 2 minutes, and 1 second, this macro will return `in 3 hours` or `3 hours ago` in your default language.

Argument | Type | Default | Example | Description
:-:|:-:|:-:|:-:|---
uptime| string, datetime, or entity_id | - | `'sensor.uptime'` | (Required) The entity_id, date string, or datetime object.
attribute| str or None | No | `None` | (Optional) attribute to extract the desired time from.
language| string | set by user | `'en'` | (Optional) Override the default language.
utc| boolean | `False` | `True` | (Optional) If your `uptime` argument does not have a timezone and you wish to treat it as a UTC timestamp, set this to True.  Otherwise the function assumes `Local` calculations.
max_period| string | `'year'` | '`hour`'| (Optional) Truncate the maximum significant period.  Available options: `year`, `week`, `day`, `hour`, `minute` and `second`

### Examples

```jinja
{% from 'easy_time.jinja' import easy_relative_time %}

{# From an entity state #}
{{ easy_relative_time('input_datetime.alarm_time') }}

{# Last Updated #}
{{ easy_relative_time(states.sensor.my_energy_meter.last_updated) }}

{# Calendars - start time #}
{# By default, easy_time macros assumes you'd like the start time #}
{{ easy_relative_time('calendar.my_events', 'hour, minute') }}
{# Calendars - end time #}
{{ easy_relative_time('calendar.my_events', 'end_time', 'hour, minute') }}

{# Overriding language or utc entity attribute #}
{{ easy_relative_time("2023-04-07 00:00:00", None, 'en', True) }}
{{ easy_relative_time("2023-04-07 00:00:00", language='en', utc=True) }}
{{ easy_relative_time("2023-04-07 00:00:00", max_period='hour') }}
{{ easy_relative_time('calendar.my_events', 'end_time', 'en', True) }}
{{ easy_relative_time('calendar.my_events', 'end_time', language='en', utc=True) }}
```

## `big_relative_time(entity_id_or_time)`

`big_relative_time` returns the friendly relative time without missing any details.  For example, if your uptime is 3 hours, 2 minutes, and 1 second, this macro will return `in 3 hours, 2 minutes and 1 second` or `3 hours, 2 minutes and 1 second ago` in your default language.

Argument | Type | Default | Example | Description
:-:|:-:|:-:|:-:|---
uptime| string, datetime, or entity_id | - | `'sensor.uptime'` | (Required) The entity_id, date string, or datetime object.
attribute| str or None | No | `None` | (Optional) attribute to extract the desired time from.
language| string | set by user | `'en'` | (Optional) Override the default language.
utc| boolean | `False` | `True` | (Optional) If your `uptime` argument does not have a timezone and you wish to treat it as a UTC timestamp, set this to True.  Otherwise the function assumes `Local` calculations.
max_period| string | `'year'` | '`hour`'| (Optional) Truncate the maximum significant period.  Available options: `year`, `week`, `day`, `hour`, `minute` and `second`

### Examples

```jinja
{% from 'easy_time.jinja' import big_relative_time %}

{# From an entity state #}
{{ big_relative_time('input_datetime.alarm_time') }}

{# Last Updated #}
{{ big_relative_time(states.sensor.my_energy_meter.last_updated) }}

{# Calendars - start time #}
{# By default, easy_time macros assumes you'd like the start time #}
{{ big_relative_time('calendar.my_events') }}
{# Calendars - end time #}
{{ big_relative_time('calendar.my_events', 'end_time') }}

{# Overriding language or utc entity attribute #}
{{ big_relative_time("2023-04-07 00:00:00", None, 'en', True) }}
{{ big_relative_time("2023-04-07 00:00:00", language='en', utc=True) }}
{{ big_relative_time("2023-04-07 00:00:00", max_period='hour') }}
{{ big_relative_time('calendar.my_events', 'end_time', 'en', True) }}
{{ big_relative_time('calendar.my_events', 'end_time', language='en', utc=True) }}
```

## `custom_relative_time(entity_id_or_time, values)` and `custom_relative_time_attr(entity_id, attribute, values)`

`custom_relative_time` and `custom_relative_time_attr` returns the friendly relative time providing details that are to your needs... most of the time.  For example, if your uptime is 3 hours, 2 minutes, and 1 second, this macro will return `3 hours and 2 minutes` in your default language.

Argument | Type | Default | Example | Description
:-:|:-:|:-:|:-:|---
uptime| string, datetime, or entity_id | - | `'sensor.uptime'` | (Required) The entity_id, date string, or datetime object.
attribute| str or None | No | `None` | (Required for `custom_relative_time_attr`) attribute to extract the desired time from.
values | string | none | `'day, hour, minute'` | (Required) Options for displaying time.  Available options: `year`, `week`, `day`, `hour`, `minute` and `second`. 
language| string | set by user | `'en'` | (Optional) Override the default language.
utc| boolean | `False` | `True` | (Optional) If your `uptime` argument does not have a timezone and you wish to treat it as a UTC timestamp, set this to True.  Otherwise the function assumes `Local` calculations.

### Examples

```jinja
{% from 'easy_time.jinja' import custom_relative_time, custom_relative_time_attr %}

{# From an entity state #}
{{ custom_relative_time('input_datetime.alarm_time', 'hour, minute') }}

{# Last Updated #}
{{ custom_relative_time(states.sensor.my_energy_meter.last_updated, 'hour, minute') }}

{# Calendars - start time #}
{# By default, easy_time macros assumes you'd like the start time #}
{{ custom_relative_time('calendar.my_events', 'hour, minute') }}
{# Calendars - end time #}
{{ custom_relative_time_attr('calendar.my_events', 'end_time', 'hour, minute') }}

{# Overriding language or utc entity attribute #}
{{ custom_relative_time("2023-04-07 00:00:00", 'hour, minute', language='en', utc=True) }}
{{ custom_relative_time_attr('calendar.my_events', 'end_time', 'hour, minute', language='en', utc=True) }}
```

# Difference Macros

These macros create times or phrases between 2 inputs.

## `easy_time_between(entity_id_or_time1, entity_id_or_time2)`

`easy_time_between` returns the most significant friendly relative time.  For example, if the time between t1 and t2 is 3 hours, 2 minutes, and 1 second, this macro will return `3 hours` in your default language.

Argument | Type | Default | Example | Description
:-:|:-:|:-:|:-:|---
entity_id_or_time1| string, datetime, or entity_id | - | `'calendar.abc'` | (Required) The first entity_id, date string, or datetime object.
entity_id_or_time2| string, datetime, or entity_id | - | `'calendar.xyz'` | (Required) The second entity_id, date string, or datetime object.
attr1| str or None | No | `None` | (Optional) attribute to extract from entity_id_or_time1
attr2| str or None | No | `None` | (Optional) attribute to extract from entity_id_or_time2
language| string | set by user | `'en'` | (Optional) Override the default language.
utc1| boolean | `False` | `True` | (Optional) If your `entity_id_or_time1` argument does not have a timezone and you wish to treat it as a UTC timestamp, set this to True.  Otherwise the function assumes `Local` calculations.
utc1| boolean | `False` | `True` | (Optional) If your `entity_id_or_time2` argument does not have a timezone and you wish to treat it as a UTC timestamp, set this to True.  Otherwise the function assumes `Local` calculations.
max_period| string | `'year'` | '`hour`'| (Optional) Truncate the maximum significant period.  Available options: `year`, `week`, `day`, `hour`, `minute` and `second`

### Examples

```jinja
{% from 'easy_time.jinja' import easy_time_between %}

{# Between 2 entities #}
{{ easy_time_between('sensor.a', 'sensor.b') }}

{# Calendars - start time #}
{# By default, easy_time_between macros assumes you'd like the start time #}
{{ easy_time_between('calendar.my_events', 'calendar.my_orther_events') }}
{# Calendars - end time #}
{{ easy_time_between('calendar.my_events', 'calendar.my_events', 'end_time', 'end_time') }}
{{ easy_time_between('calendar.my_events', 'calendar.my_events', attr1='end_time', attr2='end_time') }}
{# Duration of your calendar event #}
{{ easy_time_between('calendar.my_events', 'calendar.my_events', attr2='end_time') }}

{# Overriding language or utc entity attribute #}
{{ easy_time_between("2023-04-07 00:00:00", now(), language='en', utc=True) }}
```

## `big_time_between(entity_id_or_time1, entity_id_or_time2)`

`big_time_between` returns the most significant friendly relative time.  For example, if the time between t1 and t2 is 3 hours, 2 minutes, and 1 second, this macro will return `3 hours, 2 minutes, and 1 second` in your default language.

Argument | Type | Default | Example | Description
:-:|:-:|:-:|:-:|---
entity_id_or_time1| string, datetime, or entity_id | - | `'calendar.abc'` | (Required) The first entity_id, date string, or datetime object.
entity_id_or_time2| string, datetime, or entity_id | - | `'calendar.xyz'` | (Required) The second entity_id, date string, or datetime object.
attr1| str or None | No | `None` | (Optional) attribute to extract from entity_id_or_time1
attr2| str or None | No | `None` | (Optional) attribute to extract from entity_id_or_time2
language| string | set by user | `'en'` | (Optional) Override the default language.
utc1| boolean | `False` | `True` | (Optional) If your `entity_id_or_time1` argument does not have a timezone and you wish to treat it as a UTC timestamp, set this to True.  Otherwise the function assumes `Local` calculations.
utc1| boolean | `False` | `True` | (Optional) If your `entity_id_or_time2` argument does not have a timezone and you wish to treat it as a UTC timestamp, set this to True.  Otherwise the function assumes `Local` calculations.
max_period| string | `'year'` | '`hour`'| (Optional) Truncate the maximum significant period.  Available options: `year`, `week`, `day`, `hour`, `minute` and `second`

### Examples

```jinja
{% from 'easy_time.jinja' import big_time_between %}

{# Between 2 entities #}
{{ big_time_between('sensor.a', 'sensor.b') }}

{# Calendars - start time #}
{# By default, big_time_between macros assumes you'd like the start time #}
{{ big_time_between('calendar.my_events', 'calendar.my_orther_events') }}
{# Calendars - end time #}
{{ big_time_between('calendar.my_events', 'calendar.my_events', 'end_time', 'end_time') }}
{{ big_time_between('calendar.my_events', 'calendar.my_events', attr1='end_time', attr2='end_time') }}
{# Duration of your calendar event #}
{{ big_time_between('calendar.my_events', 'calendar.my_events', attr2='end_time') }}

{# Overriding language or utc entity attribute #}
{{ big_time_between("2023-04-07 00:00:00", now(), language='en', utc=True) }}
```

## `custom_time_between(entity_id_or_time, entity_id_or_time2, values)`

`custom_time_between` returns the friendly relative time providing details that match your needs.  For example, if your uptime is 3 hours, 2 minutes, and 1 second, this macro will return `3 hours and 2 minutes` in your default language.

Argument | Type | Default | Example | Description
:-:|:-:|:-:|:-:|---
entity_id_or_time1| string, datetime, or entity_id | - | `'calendar.abc'` | (Required) The first entity_id, date string, or datetime object.
entity_id_or_time2| string, datetime, or entity_id | - | `'calendar.xyz'` | (Required) The second entity_id, date string, or datetime object.
values | string | none | `'day, hour, minute'` | (Required) Options for displaying time.  Available options: `year`, `week`, `day`, `hour`, `minute` and `second`. 
attr1| str or None | No | `None` | (Optional) attribute to extract from entity_id_or_time1
attr2| str or None | No | `None` | (Optional) attribute to extract from entity_id_or_time2
language| string | set by user | `'en'` | (Optional) Override the default language.
utc1| boolean | `False` | `True` | (Optional) If your `entity_id_or_time1` argument does not have a timezone and you wish to treat it as a UTC timestamp, set this to True.  Otherwise the function assumes `Local` calculations.
utc1| boolean | `False` | `True` | (Optional) If your `entity_id_or_time2` argument does not have a timezone and you wish to treat it as a UTC timestamp, set this to True.  Otherwise the function assumes `Local` calculations.

### Examples

```jinja
{% from 'easy_time.jinja' import custom_time_between %}

{# Between 2 entities #}
{{ custom_time_between('sensor.a', 'sensor.b', 'hour, minute') }}

{# Calendars - start time #}
{# By default, custom_time_between macros assumes you'd like the start time #}
{{ custom_time_between('calendar.my_events', 'calendar.my_orther_events', 'hour, minute') }}
{# Calendars - end time #}
{{ custom_time_between('calendar.my_events', 'calendar.my_events', 'hour, minute', 'end_time', 'end_time') }}
{{ custom_time_between('calendar.my_events', 'calendar.my_events', 'hour, minute', attr1='end_time', attr2='end_time') }}
{# Duration of your calendar event #}
{{ custom_time_between('calendar.my_events', 'calendar.my_events', 'hour, minute', attr2='end_time') }}

{# Overriding language or utc entity attribute #}
{{ custom_time_between("2023-04-07 00:00:00", now(), 'hour, minute', language='en', utc=True) }}
```

## `time_between(entity_id_or_time1, entity_id_or_time2)`

`time_between` returns a time_delta between two entities.

Argument | Type | Default | Example | Description
:-:|:-:|:-:|:-:|---
entity_id_or_time1| string, datetime, or entity_id | - | `'calendar.abc'` | (Required) The first entity_id, date string, or datetime object.
entity_id_or_time2| string, datetime, or entity_id | - | `'calendar.xyz'` | (Required) The second entity_id, date string, or datetime object.
attr1| str or None | No | `None` | (Optional) attribute to extract from entity_id_or_time1
attr2| str or None | No | `None` | (Optional) attribute to extract from entity_id_or_time2
utc1| boolean | `False` | `True` | (Optional) If your `entity_id_or_time1` argument does not have a timezone and you wish to treat it as a UTC timestamp, set this to True.  Otherwise the function assumes `Local` calculations.
utc1| boolean | `False` | `True` | (Optional) If your `entity_id_or_time2` argument does not have a timezone and you wish to treat it as a UTC timestamp, set this to True.  Otherwise the function assumes `Local` calculations.

### Examples

```jinja
{% from 'easy_time.jinja' import time_between %}

{# Between 2 entities #}
{{ time_between('sensor.a', 'sensor.b') }}

{# Calendars - start time #}
{# By default, easy_time_between macros assumes you'd like the start time #}
{{ time_between('calendar.my_events', 'calendar.my_orther_events') }}
{# Calendars - end time #}
{{ time_between('calendar.my_events', 'calendar.my_events', 'end_time', 'end_time') }}
{{ time_between('calendar.my_events', 'calendar.my_events', attr1='end_time', attr2='end_time') }}
{# Duration of your calendar event #}
{{ time_between('calendar.my_events', 'calendar.my_events', attr2='end_time') }}
```

# Date Macros

These macros create iso formatted date strings that can easily be turned into datetime objects.


## Date Arguments

<table>
<tr><td valign="Top">

month | number
:-:|:-:
January | 1
February | 2
March | 3
April | 4
May | 5
June | 6
July | 7
August | 8
September | 9
October | 10
November | 11
December | 12

</td><td valign="Top">

week | number
:-:|:-:
1st | 1
2nd | 2
3rd | 3
4th | 4
5th | 5
6th | 6

</td><td valign="Top">

weekday | number
:-:|:-:
Monday|1
Tuesday|2
Wednesday|3
Thursday|4
Friday|5
Saturday|6
Sunday|7

</td><td valign="Top">

day | number
:-:|:-:
1st | 1
2nd | 2
3rd | 3
4th | 4
5th | 5
6th | 6
7th | 7
8th | 8
9th | 9
10th | 10
11th | 11
12th | 12
13th | 13
14th | 14
15th | 15
16th | 16
17th | 17
18th | 18
19th | 19
20th | 20
21st | 21
22nd | 22
23rd | 23
24th | 24
25th | 25
26th | 26
27th | 27
28th | 28
29th | 29
30th | 30
31st | 31
</td></tr> </table>

## `this_weekday(weekday)`, `last_weekday(weekday)`, `next_weekday(weekday)`

Get the day of the week, in the future or past.

### Examples

```jinja
{% from 'easy_time.jinja' import this_weekday, last_weekday, next_weekday %}

{# Get Tuesdays date, this week #}
{{ this_weekday(2) | as_datetime }}

{# Get Tuesdays date, next week #}
{{ next_weekday(2) | as_datetime }}

{# Get last Tuesdays date #}
{{ last_weekday(2) | as_datetime}}
```

## `month_week_day(month, week, weekday)`

This macro will return the nth day in the nth week of a month.  This is best used to get the 2nd tuesday of any month, or the Thanksgiving American Holiday!

### Examples

```jinja
{% from 'easy_time.jinja' import month_week_day %}

{# Thanksgiving as a iso string #}
{{ month_week_day(11, 4, 3) }} 

{# Thanksgiving as a datetime for math#}
{{ month_week_day(11, 4, 3) | as_datetime }} 

{# 2nd sunday in January as an ios string #}
{{ month_week_day(1, 2, 2) }} 

{# 2nd sunday in January as a datetime #}
{{ month_week_day(1, 2, 2) | as_datetime }} 
```

## `month_day(month, day)`

This macro will return the nth day in the nth week of a month.  This is best used to get the 2nd tuesday of any month, or the Thanksgiving American Holiday!

### Examples

```jinja
{% from 'easy_time.jinja' import month_day %}

{# 4th of july #}
{{ month_day(7, 4) }} 
{{ month_day(7, 4) | as_datetime }} 
```

## `last_day_in_month(month, weekday)`

This macro will return the last day in a moth.  Do you ever want the last Sunday of each month?  Look no futhur.

### Examples

```jinja
{% from 'easy_time.jinja' import last_day_in_month %}

{# Last Sunday in August #}
{{ last_day_in_month(8, 6) }} 
{{ last_day_in_month(8, 6) | as_datetime }} 
```

## `easter()`

Apparently easter falls on a different sunday ever year and it takes a small army to figure it out.

### Examples

```jinja
{% from 'easy_time.jinja' import easter %}

{# Last Sunday in August #}
{{ easter() }} 
{{ easter() | as_datetime }} 
```
# Counts

## `days_in_month(month)`, `days_next_month(offset)`, `days_last_month(offset)`

Output the number of days this month, or next month, or any month!
### Examples

```jinja
{% from 'easy_time.jinja' import days_in_month %}

{# Number of days this month #}
{{ days_in_month() | int }}

{# Number of days in december #}
{{ days_in_month(12) | int }}

{# Number of days in February #}
{# works on leap year #}
{{ days_in_month(2) | int }}

{# Number of days next month #}
{{ days_next_month() | int }}
{# Optionally add an offset for further in the future #}
{{ days_next_month(2) | int }}

{# Number of days last month #}
{{ days_last_month() | int }}
```

## `count_the_days(entity_id_or_time)`

`count_the_days` returns the number of days between now and your event.  For example, if your event is in 1 day, this macro will return `1`.

Argument | Type | Default | Example | Description
:-:|:-:|:-:|:-:|---
entity_id_or_time| string, datetime, or entity_id | - | `'sensor.uptime'` | (Required) The entity_id, date string, or datetime object.
attribute| str or None | No | `None` | (Optional) attribute to extract the desired time from.
language| string | set by user | `'en'` | (Optional) Override the default language.
utc| boolean | `False` | `True` | (Optional) If your `uptime` argument does not have a timezone and you wish to treat it as a UTC timestamp, set this to True.  Otherwise the function assumes `Local` calculations.

### Examples

```jinja
{% from 'easy_time.jinja' import count_the_days %}

{# From an entity state #}
{{ count_the_days('input_datetime.alarm_time') }}

{# Last Updated #}
{{ count_the_days(states.sensor.my_energy_meter.last_updated) }}

{# Calendars - start time #}
{# By default, easy_time macros assumes you'd like the start time #}
{{ count_the_days('calendar.my_events') }}
{# Calendars - end time #}
{{ count_the_days('calendar.my_events', 'end_time') }}

{# Overriding language or utc entity attribute #}
{{ count_the_days("2023-04-07 00:00:00", utc=True) }}
{{ count_the_days('calendar.my_events', 'end_time', True) }}
{{ count_the_days('calendar.my_events', 'end_time', utc=True) }}
```

## `speak_the_days(entity_id_or_time)`

`speak_the_days` returns the number of days between now and your event on your default language.  For example, if your event is in 1 day, this macro will return `tomorrow` in your default language.

Argument | Type | Default | Example | Description
:-:|:-:|:-:|:-:|---
entity_id_or_time| string, datetime, or entity_id | - | `'sensor.uptime'` | (Required) The entity_id, date string, or datetime object.
attribute| str or None | No | `None` | (Optional) attribute to extract the desired time from.
language| string | set by user | `'en'` | (Optional) Override the default language.
utc| boolean | `False` | `True` | (Optional) If your `uptime` argument does not have a timezone and you wish to treat it as a UTC timestamp, set this to True.  Otherwise the function assumes `Local` calculations.

### Examples

```jinja
{% from 'easy_time.jinja' import speak_the_days %}

{# From an entity state #}
{{ speak_the_days('input_datetime.alarm_time') }}

{# Last Updated #}
{{ speak_the_days(states.sensor.my_energy_meter.last_updated) }}

{# Calendars - start time #}
{# By default, easy_time macros assumes you'd like the start time #}
{{ speak_the_days('calendar.my_events') }}
{# Calendars - end time #}
{{ speak_the_days('calendar.my_events', 'end_time') }}

{# Overriding language or utc entity attribute #}
{{ speak_the_days("2023-04-07 00:00:00", 'en', True) }}
{{ speak_the_days("2023-04-07 00:00:00", language='en', utc=True) }}
{{ speak_the_days('calendar.my_events', 'end_time', 'en', True) }}
{{ speak_the_days('calendar.my_events', 'end_time', language='en', utc=True) }}
```

# Daylight Savings

Ever wonder if you're falling behind or jumping ahead?  Want to be notified a week before daylight savings?  These templates will help with that.

## `next_dst()`

Returns an iso formatted time string of the exact minute the next DST goes into affect.

### Examples

```jinja
{% from 'easy_time.jinja' import next_dst %}

{# Next daylight savings time #}
{{ next_dst() }} 
{{ next_dst() | as_datetime }} 
```

## `next_dst_phrase()`

Want a friendly phrase telling you whether you will gain or lose time?  `gain 1 hour` or `lose 1 hour`

### Examples

```jinja
{% from 'easy_time.jinja' import next_dst_phrase %}

{# Next daylight savings time #}
{{ next_dst_phrase() }} 
{{ next_dst_phrase() | as_datetime }} 
```

## `days_until_dst()`

This outputs the number of days until the next DST.  Useful for notifications.  When the number of days is 7, send a notification.  "You will gain 1 hour in 7 days"

### Examples

```jinja
{% from 'easy_time.jinja' import days_until_dst %}

{# Next daylight savings time #}
{{ days_until_dst() }} 
{{ days_until_dst() | int }}
```

# Translations


## `month(month)`

Outputs the current month in your langauge.  (Optional) Add the [month](https://github.com/Petro31/easy-time-jinja#arguments-for-each-macro) argument to get any translated month.

```jinja
{% from 'easy_time.jinja' import month %}

{# current month #}
{{ month() }}

{# December #}
{{ month(12) }}
```

## `weekday(weekday)`

Outputs the current weekday in your langauge.  (Optional) Add the [weekday](https://github.com/Petro31/easy-time-jinja#date-arguments) argument to get any translated month.

```jinja
{% from 'easy_time.jinja' import weekday %}

{# current weekday #}
{{ weekday() }}

{# Monday #}
{{ weekday(1) }}
```

# Questions & Support

For questions or support, please visit the home assistant forums [here](https://github.com/Petro31/easy-time-jinja#date-arguments)
