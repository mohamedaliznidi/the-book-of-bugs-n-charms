---
description: >-
  date-fns is a modern JavaScript date utility library providing over 200 functions 
  for manipulating, formatting, and working with dates in a functional programming style.
---

# date-fns

## Introduction

date-fns is a modern JavaScript date utility library that provides over 200 functions for manipulating, formatting, and working with dates. It follows functional programming principles, is modular, immutable, and provides excellent TypeScript support. Unlike moment.js, date-fns is tree-shakable and lightweight.

## Use Cases

1. **Date Formatting**: Format dates for display in various formats and locales
2. **Date Manipulation**: Add, subtract, and modify dates safely
3. **Date Comparison**: Compare dates and calculate differences
4. **Internationalization**: Work with dates in multiple languages and locales
5. **Business Logic**: Handle business days, working hours, and scheduling
6. **Form Validation**: Validate date inputs and ranges

## Installation

### NPM

```bash
npm install date-fns
```

### Yarn

```bash
yarn add date-fns
```

### CDN

```html
<script src="https://cdn.jsdelivr.net/npm/date-fns@2.29.3/index.min.js"></script>
```

## Basic Usage

### Importing Functions

```javascript
// Import specific functions (recommended for tree-shaking)
import { format, addDays, subDays, isAfter, parseISO } from 'date-fns';

// Import all functions (not recommended for production)
import * as dateFns from 'date-fns';

// CommonJS
const { format, addDays } = require('date-fns');
```

### Basic Date Operations

```javascript
import { format, addDays, subDays, startOfWeek, endOfWeek } from 'date-fns';

const now = new Date();

// Format dates
const formatted = format(now, 'yyyy-MM-dd'); // "2023-12-15"
const readable = format(now, 'MMMM do, yyyy'); // "December 15th, 2023"

// Add and subtract time
const tomorrow = addDays(now, 1);
const lastWeek = subDays(now, 7);

// Get week boundaries
const weekStart = startOfWeek(now);
const weekEnd = endOfWeek(now);

console.log('Today:', format(now, 'PPP'));
console.log('Tomorrow:', format(tomorrow, 'PPP'));
console.log('Week:', format(weekStart, 'PPP'), 'to', format(weekEnd, 'PPP'));
```

## Date Formatting

### Common Format Patterns

```javascript
import { format } from 'date-fns';

const date = new Date(2023, 11, 15, 14, 30, 0); // December 15, 2023, 2:30 PM

// Basic formats
format(date, 'yyyy-MM-dd');           // "2023-12-15"
format(date, 'dd/MM/yyyy');           // "15/12/2023"
format(date, 'MM-dd-yyyy');           // "12-15-2023"

// Time formats
format(date, 'HH:mm:ss');             // "14:30:00"
format(date, 'h:mm a');               // "2:30 PM"
format(date, 'HH:mm');                // "14:30"

// Combined date and time
format(date, 'yyyy-MM-dd HH:mm:ss');  // "2023-12-15 14:30:00"
format(date, 'PPP p');                // "December 15th, 2023 at 2:30 PM"

// Readable formats
format(date, 'EEEE, MMMM do, yyyy');  // "Friday, December 15th, 2023"
format(date, 'MMM d, yyyy');          // "Dec 15, 2023"
format(date, 'do MMMM yyyy');         // "15th December 2023"

// ISO formats
format(date, "yyyy-MM-dd'T'HH:mm:ss.SSSxxx"); // "2023-12-15T14:30:00.000+00:00"
```

### Predefined Format Strings

```javascript
import { format } from 'date-fns';

const date = new Date();

// Predefined formats
format(date, 'P');    // "12/15/2023" (short date)
format(date, 'PP');   // "Dec 15, 2023" (medium date)
format(date, 'PPP');  // "December 15th, 2023" (long date)
format(date, 'PPPP'); // "Friday, December 15th, 2023" (full date)

format(date, 'p');    // "2:30 PM" (short time)
format(date, 'pp');   // "2:30:00 PM" (medium time)
format(date, 'ppp');  // "2:30:00 PM GMT+0" (long time)
format(date, 'pppp'); // "2:30:00 PM GMT+00:00" (full time)

// Combined
format(date, 'Pp');   // "12/15/2023, 2:30 PM"
format(date, 'PPpp'); // "Dec 15, 2023, 2:30:00 PM"
```

## Date Manipulation

### Adding and Subtracting Time

```javascript
import { 
    addYears, addMonths, addWeeks, addDays, addHours, addMinutes, addSeconds,
    subYears, subMonths, subWeeks, subDays, subHours, subMinutes, subSeconds,
    add, sub
} from 'date-fns';

const date = new Date(2023, 11, 15); // December 15, 2023

// Add time units
const nextYear = addYears(date, 1);      // December 15, 2024
const nextMonth = addMonths(date, 3);    // March 15, 2024
const nextWeek = addWeeks(date, 2);      // December 29, 2023
const tomorrow = addDays(date, 1);       // December 16, 2023

// Subtract time units
const lastYear = subYears(date, 1);      // December 15, 2022
const lastMonth = subMonths(date, 1);    // November 15, 2023
const yesterday = subDays(date, 1);      // December 14, 2023

// Add/subtract multiple units at once
const futureDate = add(date, {
    years: 1,
    months: 2,
    days: 10,
    hours: 5,
    minutes: 30
});

const pastDate = sub(date, {
    years: 2,
    months: 6,
    days: 15
});

console.log('Future:', format(futureDate, 'PPP p'));
console.log('Past:', format(pastDate, 'PPP'));
```

### Date Boundaries

```javascript
import { 
    startOfYear, endOfYear,
    startOfMonth, endOfMonth,
    startOfWeek, endOfWeek,
    startOfDay, endOfDay,
    startOfHour, endOfHour
} from 'date-fns';

const date = new Date(2023, 11, 15, 14, 30, 45); // December 15, 2023, 2:30:45 PM

// Year boundaries
const yearStart = startOfYear(date);  // January 1, 2023, 00:00:00
const yearEnd = endOfYear(date);      // December 31, 2023, 23:59:59.999

// Month boundaries
const monthStart = startOfMonth(date); // December 1, 2023, 00:00:00
const monthEnd = endOfMonth(date);     // December 31, 2023, 23:59:59.999

// Week boundaries (starts on Sunday by default)
const weekStart = startOfWeek(date);   // December 10, 2023, 00:00:00
const weekEnd = endOfWeek(date);       // December 16, 2023, 23:59:59.999

// Day boundaries
const dayStart = startOfDay(date);     // December 15, 2023, 00:00:00
const dayEnd = endOfDay(date);         // December 15, 2023, 23:59:59.999

// Hour boundaries
const hourStart = startOfHour(date);   // December 15, 2023, 14:00:00
const hourEnd = endOfHour(date);       // December 15, 2023, 14:59:59.999

console.log('Month range:', format(monthStart, 'PPP'), 'to', format(monthEnd, 'PPP'));
```

## Date Comparison and Validation

### Comparison Functions

```javascript
import { 
    isAfter, isBefore, isEqual, isSameDay, isSameMonth, isSameYear,
    isWithinInterval, compareAsc, compareDesc,
    min, max, closestTo
} from 'date-fns';

const date1 = new Date(2023, 11, 15);
const date2 = new Date(2023, 11, 20);
const date3 = new Date(2023, 10, 15);

// Basic comparisons
console.log(isAfter(date2, date1));    // true
console.log(isBefore(date1, date2));   // true
console.log(isEqual(date1, date1));    // true

// Same period comparisons
console.log(isSameDay(date1, date1));     // true
console.log(isSameMonth(date1, date3));   // false
console.log(isSameYear(date1, date2));    // true

// Interval checking
const interval = { start: date1, end: date2 };
const testDate = new Date(2023, 11, 17);
console.log(isWithinInterval(testDate, interval)); // true

// Sorting dates
const dates = [date2, date1, date3];
const ascending = dates.sort(compareAsc);   // [date3, date1, date2]
const descending = dates.sort(compareDesc); // [date2, date1, date3]

// Min/Max dates
const earliest = min(dates);  // date3
const latest = max(dates);    // date2

// Closest date
const target = new Date(2023, 11, 16);
const closest = closestTo(target, dates); // date1
```

### Date Validation

```javascript
import { isValid, isDate, isExists, isPast, isFuture, isToday } from 'date-fns';

// Validate dates
console.log(isValid(new Date()));           // true
console.log(isValid(new Date('invalid')));  // false
console.log(isDate(new Date()));            // true
console.log(isDate('2023-12-15'));          // false

// Check if date exists (handles leap years, etc.)
console.log(isExists(2023, 1, 29));  // false (February 29, 2023 doesn't exist)
console.log(isExists(2024, 1, 29));  // true (2024 is a leap year)

// Temporal validation
const pastDate = new Date(2022, 0, 1);
const futureDate = new Date(2025, 0, 1);
const today = new Date();

console.log(isPast(pastDate));     // true
console.log(isFuture(futureDate)); // true
console.log(isToday(today));       // true
```

## Working with Intervals and Durations

### Date Intervals

```javascript
import { 
    intervalToDuration, formatDuration,
    differenceInYears, differenceInMonths, differenceInDays,
    differenceInHours, differenceInMinutes, differenceInSeconds,
    eachDayOfInterval, eachWeekOfInterval, eachMonthOfInterval
} from 'date-fns';

const startDate = new Date(2023, 0, 1);  // January 1, 2023
const endDate = new Date(2023, 11, 31);  // December 31, 2023

// Calculate duration
const duration = intervalToDuration({ start: startDate, end: endDate });
console.log(duration); // { years: 0, months: 11, days: 30, hours: 0, minutes: 0, seconds: 0 }

// Format duration
const formatted = formatDuration(duration);
console.log(formatted); // "11 months 30 days"

// Calculate specific differences
const yearsDiff = differenceInYears(endDate, startDate);     // 0
const monthsDiff = differenceInMonths(endDate, startDate);   // 11
const daysDiff = differenceInDays(endDate, startDate);       // 364

// Generate date ranges
const allDays = eachDayOfInterval({ start: startDate, end: addDays(startDate, 6) });
const allWeeks = eachWeekOfInterval({ start: startDate, end: endDate });
const allMonths = eachMonthOfInterval({ start: startDate, end: endDate });

console.log('First week days:', allDays.map(d => format(d, 'MMM d')));
console.log('All months:', allMonths.map(d => format(d, 'MMM yyyy')));
```

### Business Date Calculations

```javascript
import { 
    addBusinessDays, subBusinessDays,
    isWeekend, isMonday, isTuesday, isWednesday, isThursday, isFriday,
    nextMonday, nextFriday, previousMonday, previousFriday,
    eachWeekendOfInterval
} from 'date-fns';

const date = new Date(2023, 11, 15); // Friday, December 15, 2023

// Business day calculations
const nextBusinessDay = addBusinessDays(date, 1);     // Monday, December 18, 2023
const prevBusinessDay = subBusinessDays(date, 1);     // Thursday, December 14, 2023

// Weekend checking
console.log(isWeekend(date));           // false (Friday is not weekend)
console.log(isWeekend(addDays(date, 1))); // true (Saturday is weekend)

// Day of week checking
console.log(isFriday(date));            // true
console.log(isMonday(nextBusinessDay)); // true

// Next/Previous specific days
const nextMon = nextMonday(date);       // Monday, December 18, 2023
const prevMon = previousMonday(date);   // Monday, December 11, 2023

// Get all weekends in a period
const interval = { start: startOfMonth(date), end: endOfMonth(date) };
const weekends = eachWeekendOfInterval(interval);
console.log('Weekend days in December:', weekends.map(d => format(d, 'MMM d')));
```

## Parsing and Converting

### Parsing Dates

```javascript
import { parseISO, parse, fromUnixTime, getUnixTime } from 'date-fns';

// Parse ISO strings
const isoDate = parseISO('2023-12-15T14:30:00.000Z');
console.log(format(isoDate, 'PPP p')); // "December 15th, 2023 at 2:30 PM"

// Parse custom formats
const customDate = parse('15/12/2023', 'dd/MM/yyyy', new Date());
const timeDate = parse('2:30 PM', 'h:mm a', new Date());

// Unix timestamp conversion
const unixTimestamp = 1702651800; // Unix timestamp
const dateFromUnix = fromUnixTime(unixTimestamp);
const backToUnix = getUnixTime(dateFromUnix);

console.log('From Unix:', format(dateFromUnix, 'PPP p'));
console.log('Back to Unix:', backToUnix);
```

### Date Conversion Utilities

```javascript
import { toDate, formatISO, formatRFC3339, formatRFC7231 } from 'date-fns';

const date = new Date(2023, 11, 15, 14, 30, 0);

// Convert to different formats
const isoString = formatISO(date);           // "2023-12-15T14:30:00+00:00"
const rfc3339 = formatRFC3339(date);         // "2023-12-15T14:30:00+00:00"
const rfc7231 = formatRFC7231(date);         // "Fri, 15 Dec 2023 14:30:00 GMT"

// Safe date conversion
const safeDate = toDate(date); // Ensures it's a Date object

console.log('ISO:', isoString);
console.log('RFC 3339:', rfc3339);
console.log('RFC 7231:', rfc7231);

## Internationalization (i18n)

### Using Locales

```javascript
import { format, formatDistance, formatRelative } from 'date-fns';
import { es, fr, de, ja, ar } from 'date-fns/locale';

const date = new Date(2023, 11, 15, 14, 30, 0);
const baseDate = new Date(2023, 11, 10);

// Format with different locales
console.log(format(date, 'PPPP', { locale: es }));    // "viernes, 15 de diciembre de 2023"
console.log(format(date, 'PPPP', { locale: fr }));    // "vendredi 15 décembre 2023"
console.log(format(date, 'PPPP', { locale: de }));    // "Freitag, 15. Dezember 2023"
console.log(format(date, 'PPPP', { locale: ja }));    // "2023年12月15日金曜日"

// Relative time with locales
console.log(formatDistance(date, baseDate, { locale: es })); // "5 días"
console.log(formatDistance(date, baseDate, { locale: fr })); // "5 jours"
console.log(formatDistance(date, baseDate, { locale: de })); // "5 Tage"

// Relative formatting
console.log(formatRelative(date, baseDate, { locale: es })); // "el viernes pasado a las 14:30"
console.log(formatRelative(date, baseDate, { locale: fr })); // "vendredi dernier à 14:30"
```

### Custom Locale Configuration

```javascript
import { format } from 'date-fns';
import { enUS } from 'date-fns/locale';

// Create custom locale with modified options
const customLocale = {
  ...enUS,
  options: {
    ...enUS.options,
    weekStartsOn: 1, // Monday
    firstWeekContainsDate: 4
  }
};

// Use custom locale
const date = new Date(2023, 11, 15);
console.log(format(date, 'PPPP', { locale: customLocale }));
```

### Multi-language Date Formatter

```javascript
class MultiLanguageDateFormatter {
  constructor() {
    this.locales = new Map([
      ['en', enUS],
      ['es', es],
      ['fr', fr],
      ['de', de],
      ['ja', ja]
    ]);
    this.currentLocale = 'en';
  }

  setLocale(locale) {
    if (this.locales.has(locale)) {
      this.currentLocale = locale;
    } else {
      console.warn(`Locale ${locale} not supported`);
    }
  }

  format(date, formatString) {
    const locale = this.locales.get(this.currentLocale);
    return format(date, formatString, { locale });
  }

  formatRelative(date, baseDate = new Date()) {
    const locale = this.locales.get(this.currentLocale);
    return formatRelative(date, baseDate, { locale });
  }

  formatDistance(date, baseDate = new Date()) {
    const locale = this.locales.get(this.currentLocale);
    return formatDistance(date, baseDate, { locale, addSuffix: true });
  }

  getAvailableLocales() {
    return Array.from(this.locales.keys());
  }
}

// Usage
const formatter = new MultiLanguageDateFormatter();
const date = new Date(2023, 11, 15);

formatter.setLocale('es');
console.log(formatter.format(date, 'PPPP'));
console.log(formatter.formatDistance(subDays(date, 3)));

formatter.setLocale('fr');
console.log(formatter.format(date, 'PPPP'));
console.log(formatter.formatRelative(addDays(date, 2)));
```

## Timezone Handling

### Working with Timezones (date-fns-tz)

```javascript
// Note: Requires separate installation: npm install date-fns-tz
import { zonedTimeToUtc, utcToZonedTime, format as formatTz } from 'date-fns-tz';

const date = new Date('2023-12-15T14:30:00');
const timeZone = 'America/New_York';

// Convert local time to UTC
const utcDate = zonedTimeToUtc(date, timeZone);

// Convert UTC to specific timezone
const zonedDate = utcToZonedTime(utcDate, 'Europe/London');

// Format with timezone
const formatted = formatTz(zonedDate, 'yyyy-MM-dd HH:mm:ss zzz', {
  timeZone: 'Europe/London'
});

console.log('Original:', date);
console.log('UTC:', utcDate);
console.log('London time:', formatted);
```

### Timezone-aware Date Manager

```javascript
class TimezoneManager {
  constructor(defaultTimezone = 'UTC') {
    this.defaultTimezone = defaultTimezone;
    this.supportedTimezones = [
      'UTC',
      'America/New_York',
      'America/Los_Angeles',
      'Europe/London',
      'Europe/Paris',
      'Asia/Tokyo',
      'Asia/Shanghai',
      'Australia/Sydney'
    ];
  }

  convertToTimezone(date, targetTimezone) {
    if (!this.supportedTimezones.includes(targetTimezone)) {
      throw new Error(`Unsupported timezone: ${targetTimezone}`);
    }

    return utcToZonedTime(date, targetTimezone);
  }

  formatInTimezone(date, formatString, timezone) {
    return formatTz(date, formatString, { timeZone: timezone });
  }

  getCurrentTimeInTimezone(timezone) {
    const now = new Date();
    return this.convertToTimezone(now, timezone);
  }

  getTimezoneOffset(timezone) {
    const date = new Date();
    const utcDate = new Date(date.getTime() + (date.getTimezoneOffset() * 60000));
    const targetDate = this.convertToTimezone(utcDate, timezone);
    return (targetDate.getTime() - utcDate.getTime()) / (1000 * 60 * 60);
  }

  compareTimesAcrossTimezones(date1, tz1, date2, tz2) {
    const utc1 = zonedTimeToUtc(date1, tz1);
    const utc2 = zonedTimeToUtc(date2, tz2);
    return compareAsc(utc1, utc2);
  }
}

// Usage
const tzManager = new TimezoneManager();
const date = new Date();

console.log('NYC:', tzManager.formatInTimezone(date, 'PPP p', 'America/New_York'));
console.log('Tokyo:', tzManager.formatInTimezone(date, 'PPP p', 'Asia/Tokyo'));
console.log('London:', tzManager.formatInTimezone(date, 'PPP p', 'Europe/London'));
```

## Practical Examples

### Date Range Picker Logic

```javascript
import {
  startOfMonth, endOfMonth, eachDayOfInterval,
  isSameMonth, isToday, isWithinInterval,
  format, addMonths, subMonths
} from 'date-fns';

class DateRangePicker {
  constructor() {
    this.currentMonth = new Date();
    this.selectedRange = { start: null, end: null };
    this.hoveredDate = null;
  }

  generateCalendarDays(month = this.currentMonth) {
    const start = startOfMonth(month);
    const end = endOfMonth(month);

    return eachDayOfInterval({ start, end }).map(date => ({
      date,
      isCurrentMonth: isSameMonth(date, month),
      isToday: isToday(date),
      isSelected: this.isDateSelected(date),
      isInRange: this.isDateInRange(date),
      isRangeStart: this.isRangeStart(date),
      isRangeEnd: this.isRangeEnd(date),
      formatted: format(date, 'd')
    }));
  }

  selectDate(date) {
    if (!this.selectedRange.start || this.selectedRange.end) {
      // Start new selection
      this.selectedRange = { start: date, end: null };
    } else {
      // Complete selection
      if (isAfter(date, this.selectedRange.start)) {
        this.selectedRange.end = date;
      } else {
        this.selectedRange = { start: date, end: this.selectedRange.start };
      }
    }
  }

  isDateSelected(date) {
    return (this.selectedRange.start && isSameDay(date, this.selectedRange.start)) ||
           (this.selectedRange.end && isSameDay(date, this.selectedRange.end));
  }

  isDateInRange(date) {
    if (!this.selectedRange.start) return false;

    const end = this.selectedRange.end || this.hoveredDate;
    if (!end) return false;

    const start = this.selectedRange.start;
    return isWithinInterval(date, {
      start: isBefore(start, end) ? start : end,
      end: isBefore(start, end) ? end : start
    });
  }

  isRangeStart(date) {
    return this.selectedRange.start && isSameDay(date, this.selectedRange.start);
  }

  isRangeEnd(date) {
    return this.selectedRange.end && isSameDay(date, this.selectedRange.end);
  }

  nextMonth() {
    this.currentMonth = addMonths(this.currentMonth, 1);
  }

  previousMonth() {
    this.currentMonth = subMonths(this.currentMonth, 1);
  }

  getSelectedRange() {
    if (!this.selectedRange.start || !this.selectedRange.end) {
      return null;
    }

    return {
      start: this.selectedRange.start,
      end: this.selectedRange.end,
      duration: intervalToDuration({
        start: this.selectedRange.start,
        end: this.selectedRange.end
      }),
      formatted: {
        start: format(this.selectedRange.start, 'PPP'),
        end: format(this.selectedRange.end, 'PPP')
      }
    };
  }

  setHoveredDate(date) {
    this.hoveredDate = date;
  }

  clearSelection() {
    this.selectedRange = { start: null, end: null };
    this.hoveredDate = null;
  }
}

// Usage
const datePicker = new DateRangePicker();
const calendarDays = datePicker.generateCalendarDays();

// Simulate user interactions
datePicker.selectDate(new Date(2023, 11, 15));
datePicker.selectDate(new Date(2023, 11, 20));

const selectedRange = datePicker.getSelectedRange();
console.log('Selected range:', selectedRange);
```

### Event Scheduling System

```javascript
class EventScheduler {
  constructor() {
    this.events = [];
    this.workingHours = { start: 9, end: 17 }; // 9 AM to 5 PM
    this.workingDays = [1, 2, 3, 4, 5]; // Monday to Friday
  }

  addEvent(title, startDate, endDate, options = {}) {
    const event = {
      id: Date.now().toString(),
      title,
      startDate: new Date(startDate),
      endDate: new Date(endDate),
      isRecurring: options.isRecurring || false,
      recurrenceRule: options.recurrenceRule || null,
      reminders: options.reminders || [],
      attendees: options.attendees || []
    };

    if (this.isValidEvent(event)) {
      this.events.push(event);
      return event;
    } else {
      throw new Error('Invalid event: conflicts with existing event or outside working hours');
    }
  }

  isValidEvent(newEvent) {
    // Check if within working hours
    if (!this.isWithinWorkingHours(newEvent.startDate, newEvent.endDate)) {
      return false;
    }

    // Check for conflicts
    return !this.hasConflict(newEvent);
  }

  isWithinWorkingHours(startDate, endDate) {
    const startHour = getHours(startDate);
    const endHour = getHours(endDate);
    const dayOfWeek = getDay(startDate);

    return this.workingDays.includes(dayOfWeek) &&
           startHour >= this.workingHours.start &&
           endHour <= this.workingHours.end;
  }

  hasConflict(newEvent) {
    return this.events.some(existingEvent => {
      return this.eventsOverlap(newEvent, existingEvent);
    });
  }

  eventsOverlap(event1, event2) {
    return isBefore(event1.startDate, event2.endDate) &&
           isAfter(event1.endDate, event2.startDate);
  }

  getEventsForDate(date) {
    return this.events.filter(event => {
      return isSameDay(event.startDate, date) ||
             (isBefore(event.startDate, date) && isAfter(event.endDate, date));
    });
  }

  getEventsInRange(startDate, endDate) {
    return this.events.filter(event => {
      return isWithinInterval(event.startDate, { start: startDate, end: endDate }) ||
             isWithinInterval(event.endDate, { start: startDate, end: endDate }) ||
             (isBefore(event.startDate, startDate) && isAfter(event.endDate, endDate));
    });
  }

  findAvailableSlots(date, durationMinutes) {
    const dayStart = setHours(setMinutes(startOfDay(date), 0), this.workingHours.start);
    const dayEnd = setHours(setMinutes(startOfDay(date), 0), this.workingHours.end);

    const dayEvents = this.getEventsForDate(date)
      .sort((a, b) => compareAsc(a.startDate, b.startDate));

    const availableSlots = [];
    let currentTime = dayStart;

    for (const event of dayEvents) {
      if (differenceInMinutes(event.startDate, currentTime) >= durationMinutes) {
        availableSlots.push({
          start: currentTime,
          end: event.startDate,
          duration: differenceInMinutes(event.startDate, currentTime)
        });
      }
      currentTime = event.endDate;
    }

    // Check remaining time after last event
    if (differenceInMinutes(dayEnd, currentTime) >= durationMinutes) {
      availableSlots.push({
        start: currentTime,
        end: dayEnd,
        duration: differenceInMinutes(dayEnd, currentTime)
      });
    }

    return availableSlots;
  }

  generateRecurringEvents(baseEvent, endDate) {
    if (!baseEvent.isRecurring || !baseEvent.recurrenceRule) {
      return [baseEvent];
    }

    const events = [baseEvent];
    const { frequency, interval = 1 } = baseEvent.recurrenceRule;
    let currentDate = baseEvent.startDate;

    while (isBefore(currentDate, endDate)) {
      switch (frequency) {
        case 'daily':
          currentDate = addDays(currentDate, interval);
          break;
        case 'weekly':
          currentDate = addWeeks(currentDate, interval);
          break;
        case 'monthly':
          currentDate = addMonths(currentDate, interval);
          break;
        case 'yearly':
          currentDate = addYears(currentDate, interval);
          break;
      }

      if (isBefore(currentDate, endDate)) {
        const duration = differenceInMinutes(baseEvent.endDate, baseEvent.startDate);
        events.push({
          ...baseEvent,
          id: `${baseEvent.id}-${format(currentDate, 'yyyy-MM-dd')}`,
          startDate: currentDate,
          endDate: addMinutes(currentDate, duration)
        });
      }
    }

    return events;
  }

  getUpcomingEvents(days = 7) {
    const now = new Date();
    const futureDate = addDays(now, days);

    return this.getEventsInRange(now, futureDate)
      .sort((a, b) => compareAsc(a.startDate, b.startDate))
      .map(event => ({
        ...event,
        timeUntil: formatDistance(event.startDate, now, { addSuffix: true }),
        formatted: {
          start: format(event.startDate, 'PPP p'),
          end: format(event.endDate, 'p')
        }
      }));
  }

  exportToCalendar(events = this.events) {
    return events.map(event => ({
      title: event.title,
      start: formatISO(event.startDate),
      end: formatISO(event.endDate),
      description: `Event: ${event.title}`,
      attendees: event.attendees.join(', ')
    }));
  }
}

// Usage
const scheduler = new EventScheduler();

// Add events
scheduler.addEvent(
  'Team Meeting',
  new Date(2023, 11, 15, 10, 0),
  new Date(2023, 11, 15, 11, 0),
  {
    isRecurring: true,
    recurrenceRule: { frequency: 'weekly', interval: 1 },
    attendees: ['john@example.com', 'jane@example.com']
  }
);

// Find available slots
const availableSlots = scheduler.findAvailableSlots(new Date(2023, 11, 15), 60);
console.log('Available 1-hour slots:', availableSlots);

// Get upcoming events
const upcoming = scheduler.getUpcomingEvents();
console.log('Upcoming events:', upcoming);

## Form Validation with Dates

### Date Input Validation

```javascript
import {
  isValid, isPast, isFuture, isAfter, isBefore,
  differenceInYears, differenceInDays, parse
} from 'date-fns';

class DateValidator {
  static validateDateString(dateString, format = 'yyyy-MM-dd') {
    try {
      const parsedDate = parse(dateString, format, new Date());
      return {
        isValid: isValid(parsedDate),
        date: parsedDate,
        errors: []
      };
    } catch (error) {
      return {
        isValid: false,
        date: null,
        errors: ['Invalid date format']
      };
    }
  }

  static validateAge(birthDate, minAge = 0, maxAge = 120) {
    const errors = [];
    const today = new Date();

    if (!isValid(birthDate)) {
      errors.push('Invalid birth date');
      return { isValid: false, errors };
    }

    if (isFuture(birthDate)) {
      errors.push('Birth date cannot be in the future');
    }

    const age = differenceInYears(today, birthDate);

    if (age < minAge) {
      errors.push(`Age must be at least ${minAge} years`);
    }

    if (age > maxAge) {
      errors.push(`Age cannot exceed ${maxAge} years`);
    }

    return {
      isValid: errors.length === 0,
      age,
      errors
    };
  }

  static validateDateRange(startDate, endDate, options = {}) {
    const errors = [];
    const {
      minDuration = 0, // in days
      maxDuration = 365, // in days
      allowSameDay = true,
      mustBeFuture = false
    } = options;

    if (!isValid(startDate)) {
      errors.push('Invalid start date');
    }

    if (!isValid(endDate)) {
      errors.push('Invalid end date');
    }

    if (errors.length > 0) {
      return { isValid: false, errors };
    }

    if (isAfter(startDate, endDate)) {
      errors.push('Start date must be before end date');
    }

    if (!allowSameDay && isSameDay(startDate, endDate)) {
      errors.push('Start and end dates cannot be the same');
    }

    if (mustBeFuture && (isPast(startDate) || isPast(endDate))) {
      errors.push('Dates must be in the future');
    }

    const duration = differenceInDays(endDate, startDate);

    if (duration < minDuration) {
      errors.push(`Duration must be at least ${minDuration} days`);
    }

    if (duration > maxDuration) {
      errors.push(`Duration cannot exceed ${maxDuration} days`);
    }

    return {
      isValid: errors.length === 0,
      duration,
      errors
    };
  }

  static validateBusinessDate(date, excludeWeekends = true, excludeHolidays = []) {
    const errors = [];

    if (!isValid(date)) {
      errors.push('Invalid date');
      return { isValid: false, errors };
    }

    if (excludeWeekends && isWeekend(date)) {
      errors.push('Date cannot be on weekend');
    }

    const isHoliday = excludeHolidays.some(holiday =>
      isSameDay(date, holiday)
    );

    if (isHoliday) {
      errors.push('Date cannot be on a holiday');
    }

    return {
      isValid: errors.length === 0,
      errors
    };
  }
}

// Usage examples
const birthDateValidation = DateValidator.validateAge(
  new Date(1990, 5, 15),
  18,
  65
);
console.log('Age validation:', birthDateValidation);

const rangeValidation = DateValidator.validateDateRange(
  new Date(2023, 11, 15),
  new Date(2023, 11, 20),
  { minDuration: 1, maxDuration: 30, mustBeFuture: true }
);
console.log('Range validation:', rangeValidation);
```

### Form Integration Example

```javascript
class DateForm {
  constructor(formElement) {
    this.form = formElement;
    this.validators = new Map();
    this.setupEventListeners();
  }

  addDateField(fieldName, validationRules = {}) {
    this.validators.set(fieldName, validationRules);
  }

  setupEventListeners() {
    this.form.addEventListener('submit', (e) => {
      e.preventDefault();
      this.validateForm();
    });

    // Real-time validation
    this.form.addEventListener('input', (e) => {
      if (e.target.type === 'date') {
        this.validateField(e.target.name, e.target.value);
      }
    });
  }

  validateField(fieldName, value) {
    const rules = this.validators.get(fieldName);
    if (!rules) return { isValid: true, errors: [] };

    let result = { isValid: true, errors: [] };

    if (rules.required && !value) {
      result = { isValid: false, errors: ['This field is required'] };
    } else if (value) {
      const date = new Date(value);

      if (rules.minAge !== undefined) {
        const ageValidation = DateValidator.validateAge(date, rules.minAge, rules.maxAge);
        if (!ageValidation.isValid) {
          result = ageValidation;
        }
      }

      if (rules.mustBeFuture && isPast(date)) {
        result.isValid = false;
        result.errors.push('Date must be in the future');
      }

      if (rules.mustBePast && isFuture(date)) {
        result.isValid = false;
        result.errors.push('Date must be in the past');
      }
    }

    this.displayFieldValidation(fieldName, result);
    return result;
  }

  validateForm() {
    const formData = new FormData(this.form);
    let isFormValid = true;
    const results = {};

    for (const [fieldName] of this.validators) {
      const value = formData.get(fieldName);
      const result = this.validateField(fieldName, value);
      results[fieldName] = result;

      if (!result.isValid) {
        isFormValid = false;
      }
    }

    if (isFormValid) {
      this.onFormValid(formData);
    } else {
      this.onFormInvalid(results);
    }

    return { isValid: isFormValid, results };
  }

  displayFieldValidation(fieldName, result) {
    const field = this.form.querySelector(`[name="${fieldName}"]`);
    const errorContainer = this.form.querySelector(`#${fieldName}-errors`);

    if (field) {
      field.classList.toggle('invalid', !result.isValid);
      field.classList.toggle('valid', result.isValid);
    }

    if (errorContainer) {
      errorContainer.innerHTML = result.errors
        .map(error => `<div class="error">${error}</div>`)
        .join('');
    }
  }

  onFormValid(formData) {
    console.log('Form is valid:', Object.fromEntries(formData));
  }

  onFormInvalid(results) {
    console.log('Form validation failed:', results);
  }
}

// HTML setup example:
/*
<form id="date-form">
  <div>
    <label for="birthdate">Birth Date:</label>
    <input type="date" name="birthdate" id="birthdate">
    <div id="birthdate-errors"></div>
  </div>

  <div>
    <label for="start-date">Start Date:</label>
    <input type="date" name="start-date" id="start-date">
    <div id="start-date-errors"></div>
  </div>

  <button type="submit">Submit</button>
</form>
*/

// JavaScript usage:
const form = new DateForm(document.getElementById('date-form'));
form.addDateField('birthdate', { required: true, minAge: 18, maxAge: 65 });
form.addDateField('start-date', { required: true, mustBeFuture: true });
```

## Best Practices

### Performance Optimization

```javascript
// 1. Import only what you need (tree-shaking)
import { format, addDays } from 'date-fns'; // ✅ Good
import * as dateFns from 'date-fns'; // ❌ Avoid

// 2. Reuse date objects when possible
const baseDate = new Date();
const tomorrow = addDays(baseDate, 1);
const nextWeek = addDays(baseDate, 7); // ✅ Reuse baseDate

// 3. Cache formatted dates for repeated use
class DateCache {
  constructor() {
    this.cache = new Map();
  }

  format(date, formatString, locale) {
    const key = `${date.getTime()}-${formatString}-${locale?.code || 'en'}`;

    if (!this.cache.has(key)) {
      const formatted = format(date, formatString, { locale });
      this.cache.set(key, formatted);
    }

    return this.cache.get(key);
  }

  clear() {
    this.cache.clear();
  }
}

// 4. Use constants for repeated operations
const MILLISECONDS_IN_DAY = 24 * 60 * 60 * 1000;
const COMMON_FORMATS = {
  SHORT_DATE: 'MM/dd/yyyy',
  LONG_DATE: 'MMMM do, yyyy',
  TIME_12H: 'h:mm a',
  TIME_24H: 'HH:mm',
  ISO_DATE: "yyyy-MM-dd'T'HH:mm:ss.SSSxxx"
};
```

### Error Handling

```javascript
// Always validate dates before operations
function safeDateOperation(date, operation) {
  if (!isValid(date)) {
    throw new Error('Invalid date provided');
  }

  try {
    return operation(date);
  } catch (error) {
    console.error('Date operation failed:', error);
    return null;
  }
}

// Graceful fallbacks
function formatDateSafely(date, formatString, fallback = 'Invalid Date') {
  try {
    if (!isValid(date)) {
      return fallback;
    }
    return format(date, formatString);
  } catch (error) {
    console.warn('Date formatting failed:', error);
    return fallback;
  }
}

// Input sanitization
function parseUserDate(input, expectedFormat = 'yyyy-MM-dd') {
  if (!input || typeof input !== 'string') {
    return { success: false, error: 'Invalid input' };
  }

  try {
    const parsed = parse(input.trim(), expectedFormat, new Date());

    if (!isValid(parsed)) {
      return { success: false, error: 'Invalid date format' };
    }

    return { success: true, date: parsed };
  } catch (error) {
    return { success: false, error: error.message };
  }
}
```

### Code Organization

```javascript
// Create utility modules for common operations
export const DateUtils = {
  // Formatting utilities
  formatters: {
    shortDate: (date) => format(date, 'MM/dd/yyyy'),
    longDate: (date) => format(date, 'MMMM do, yyyy'),
    time12h: (date) => format(date, 'h:mm a'),
    time24h: (date) => format(date, 'HH:mm'),
    iso: (date) => formatISO(date)
  },

  // Validation utilities
  validators: {
    isBusinessDay: (date) => !isWeekend(date),
    isInRange: (date, start, end) => isWithinInterval(date, { start, end }),
    isValidAge: (birthDate, minAge, maxAge) => {
      const age = differenceInYears(new Date(), birthDate);
      return age >= minAge && age <= maxAge;
    }
  },

  // Calculation utilities
  calculators: {
    businessDaysBetween: (start, end) => {
      const days = eachDayOfInterval({ start, end });
      return days.filter(day => !isWeekend(day)).length;
    },

    ageInYears: (birthDate) => differenceInYears(new Date(), birthDate),

    timeUntil: (targetDate) => formatDistance(targetDate, new Date(), { addSuffix: true })
  }
};
```

## Common Pitfalls

### Issue 1: Timezone Confusion
JavaScript Date objects are timezone-aware but can cause confusion.

**Solution:**
```javascript
// Always be explicit about timezones
import { zonedTimeToUtc, utcToZonedTime } from 'date-fns-tz';

// When working with user input, consider their timezone
function parseUserDateTime(dateString, userTimezone) {
  const localDate = parseISO(dateString);
  return zonedTimeToUtc(localDate, userTimezone);
}
```

### Issue 2: Mutating Date Objects
JavaScript Date objects are mutable, which can cause unexpected behavior.

**Solution:**
```javascript
// date-fns functions are pure and don't mutate
const originalDate = new Date();
const newDate = addDays(originalDate, 1); // originalDate is unchanged ✅

// Avoid direct Date mutations
originalDate.setDate(originalDate.getDate() + 1); // ❌ Mutates original
```

### Issue 3: Locale Loading Issues
Not properly loading locales can cause runtime errors.

**Solution:**
```javascript
// Dynamically import locales when needed
async function formatWithLocale(date, formatString, localeCode) {
  try {
    const locale = await import(`date-fns/locale/${localeCode}/index.js`);
    return format(date, formatString, { locale: locale.default });
  } catch (error) {
    console.warn(`Locale ${localeCode} not found, using default`);
    return format(date, formatString);
  }
}
```

### Issue 4: Performance with Large Date Ranges
Generating large date ranges can be memory intensive.

**Solution:**
```javascript
// Use generators for large date ranges
function* dateRange(start, end, step = { days: 1 }) {
  let current = start;
  while (isBefore(current, end) || isEqual(current, end)) {
    yield current;
    current = add(current, step);
  }
}

// Usage
for (const date of dateRange(startDate, endDate)) {
  // Process one date at a time
  console.log(format(date, 'yyyy-MM-dd'));
}
```

## References

{% embed url="https://date-fns.org/" %}

{% embed url="https://date-fns.org/docs/Getting-Started" %}

{% embed url="https://github.com/date-fns/date-fns" %}
```
```
