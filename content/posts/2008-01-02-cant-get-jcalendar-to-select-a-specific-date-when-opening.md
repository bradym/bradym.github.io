---
tags:
- Misc
date: "2008-01-02T00:00:00Z"
description: How to get around the bug that in some versions of JCalendar prevents
  the initial date from being set.
keywords: javascript, jcalendar
title: Can't get JCalendar to select a specific date when opening
url: /misc/cant-get-jcalendar-to-select-a-specific-date-when-opening.html
---
There is an error in calendar-setup.js that prevents the setting of an
initial date for the calendar. Go to line 159 and replace the code:

{{< highlight js >}}
if (dateEl) params.date = Date.parseDate(dateEl.value || dateEl.innerHTML, dateFmt);
```
with:
{{< highlight js >}}
if (dateEl && (dateEl.value || dateEl.innerHTML)) params.date = Date.parseDate(dateEl.value || dateEl.innerHTML, dateFmt);
```
