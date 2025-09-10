---
description: >-
  Moment.js is a legacy JavaScript library for parsing, validating, manipulating,
  and formatting dates. Note: Moment.js is now in maintenance mode.

theme: "magic"
---

# The Moment.js Legacy Temporal Sorcery Grimoire

## The Ancient Knowledge

Moment.js is a legacy JavaScript library that was widely used for parsing, validating, manipulating, and formatting dates through powerful temporal enchantments. While it provided an excellent API and was very popular among time sorcerers, the Moment.js team has put the project in maintenance mode and recommends using modern alternatives like date-fns, Day.js, or Luxon for new mystical projects.

**⚠️ Important Prophecy**: Moment.js is now in maintenance mode. For new projects, consider using these modern temporal libraries:
- **date-fns**: Modular and tree-shakable temporal spells
- **Day.js**: Moment.js-compatible API with smaller bundle size enchantments
- **Luxon**: Modern replacement built by Moment.js team with enhanced temporal powers
- **Native JavaScript Date**: For simple temporal manipulation use cases

## When to Cast These Spells (Legacy Projects)

1. **Date Parsing Rituals**: Parse dates from various string formats through temporal interpretation magic
2. **Date Formatting Enchantments**: Display dates in different formats and locales using presentation spells
3. **Date Manipulation Sorcery**: Add, subtract, and modify dates through temporal transformation magic
4. **Relative Time Divination**: Display human-readable relative times with chronological insights
5. **Timezone Handling Mysticism**: Work with different timezones through geographical temporal magic
6. **Date Validation Prophecy**: Validate date inputs and ranges with protective temporal barriers

## Summoning the Legacy Temporal Spirits

### CDN Invocation

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.29.4/moment.min.js"></script>
<script>
    // Moment is available globally
    const now = moment();
    console.log(now.format('YYYY-MM-DD'));
</script>
```

### NPM Ritual (Legacy Projects Only)

```bash
npm install moment
```

```javascript
// CommonJS
const moment = require('moment');

// ES6 Modules
import moment from 'moment';

// Basic usage
const now = moment();
console.log(now.format('YYYY-MM-DD HH:mm:ss'));
```

## Basic Temporal Usage Rituals

### Creating Moment Object Manifestations

```javascript
// Current date and time
const now = moment();

// From string (ISO 8601)
const fromString = moment('2023-12-15');
const fromDateTime = moment('2023-12-15T14:30:00');

// From string with format
const customFormat = moment('15/12/2023', 'DD/MM/YYYY');
const timeFormat = moment('2:30 PM', 'h:mm A');

// From JavaScript Date
const fromDate = moment(new Date());

// From timestamp
const fromTimestamp = moment(1702651800000);

// From array [year, month, day, hour, minute, second, millisecond]
// Note: month is 0-indexed
const fromArray = moment([2023, 11, 15, 14, 30, 0]);

// From object
const fromObject = moment({
    year: 2023,
    month: 11, // 0-indexed
    day: 15,
    hour: 14,
    minute: 30,
    second: 0
});

// Clone moment object
const cloned = moment(now);
const cloned2 = now.clone();

console.log('Now:', now.format());
console.log('From string:', fromString.format());
console.log('Custom format:', customFormat.format());
```

### Validation Prophecies

```javascript
// Check if moment is valid
const validDate = moment('2023-12-15');
const invalidDate = moment('invalid-date');

console.log(validDate.isValid()); // true
console.log(invalidDate.isValid()); // false

// Strict parsing
const strictValid = moment('2023-12-15', 'YYYY-MM-DD', true);
const strictInvalid = moment('15/12/2023', 'YYYY-MM-DD', true);

console.log(strictValid.isValid()); // true
console.log(strictInvalid.isValid()); // false

// Check parsing info
const parseInfo = moment('2023-12-15').parsingFlags();
console.log('Parsing flags:', parseInfo);
```

## Date Formatting Enchantments

### Common Format Pattern Spells

```javascript
const date = moment('2023-12-15T14:30:45.123');

// Basic formats
console.log(date.format('YYYY-MM-DD'));           // 2023-12-15
console.log(date.format('DD/MM/YYYY'));           // 15/12/2023
console.log(date.format('MM-DD-YYYY'));           // 12-15-2023

// Time formats
console.log(date.format('HH:mm:ss'));             // 14:30:45
console.log(date.format('h:mm A'));               // 2:30 PM
console.log(date.format('h:mm:ss A'));            // 2:30:45 PM

// Combined date and time
console.log(date.format('YYYY-MM-DD HH:mm:ss'));  // 2023-12-15 14:30:45
console.log(date.format('MMM D, YYYY h:mm A'));   // Dec 15, 2023 2:30 PM

// Readable formats
console.log(date.format('dddd, MMMM Do YYYY'));   // Friday, December 15th 2023
console.log(date.format('ddd MMM D, YYYY'));      // Fri Dec 15, 2023

// ISO formats
console.log(date.toISOString());                  // 2023-12-15T14:30:45.123Z
console.log(date.format());                       // 2023-12-15T14:30:45+00:00

// Custom formats
console.log(date.format('[Today is] dddd'));      // Today is Friday
console.log(date.format('YYYY [escaped] YYYY'));  // 2023 escaped 2023
```

### Localized Formatting Mysticism

```javascript
// Set global locale
moment.locale('es'); // Spanish
console.log(moment().format('LLLL')); // viernes, 15 de diciembre de 2023 14:30

moment.locale('fr'); // French
console.log(moment().format('LLLL')); // vendredi 15 décembre 2023 14:30

moment.locale('de'); // German
console.log(moment().format('LLLL')); // Freitag, 15. Dezember 2023 14:30

// Reset to English
moment.locale('en');

// Use locale for specific instance
const spanishDate = moment().locale('es');
console.log(spanishDate.format('LLLL'));

// Predefined locale formats
console.log(moment().format('L'));    // 12/15/2023
console.log(moment().format('LL'));   // December 15, 2023
console.log(moment().format('LLL'));  // December 15, 2023 2:30 PM
console.log(moment().format('LLLL')); // Friday, December 15, 2023 2:30 PM
```

## Date Manipulation Sorcery

### Adding and Subtracting Time Spells

```javascript
const date = moment('2023-12-15T14:30:00');

// Add time units
const nextYear = date.clone().add(1, 'year');
const nextMonth = date.clone().add(3, 'months');
const nextWeek = date.clone().add(2, 'weeks');
const tomorrow = date.clone().add(1, 'day');
const nextHour = date.clone().add(1, 'hour');

// Subtract time units
const lastYear = date.clone().subtract(1, 'year');
const lastMonth = date.clone().subtract(1, 'month');
const yesterday = date.clone().subtract(1, 'day');

// Add multiple units
const future = date.clone().add({
    years: 1,
    months: 2,
    days: 10,
    hours: 5,
    minutes: 30
});

// Chaining operations
const result = moment()
    .add(1, 'year')
    .subtract(2, 'months')
    .add(15, 'days')
    .startOf('day');

console.log('Original:', date.format());
console.log('Next year:', nextYear.format());
console.log('Future:', future.format());
console.log('Chained result:', result.format());
```

### Start and End of Time Period Boundaries

```javascript
const date = moment('2023-12-15T14:30:45');

// Start of periods
console.log('Start of year:', date.clone().startOf('year').format());
console.log('Start of month:', date.clone().startOf('month').format());
console.log('Start of week:', date.clone().startOf('week').format());
console.log('Start of day:', date.clone().startOf('day').format());
console.log('Start of hour:', date.clone().startOf('hour').format());

// End of periods
console.log('End of year:', date.clone().endOf('year').format());
console.log('End of month:', date.clone().endOf('month').format());
console.log('End of week:', date.clone().endOf('week').format());
console.log('End of day:', date.clone().endOf('day').format());
console.log('End of hour:', date.clone().endOf('hour').format());

// Useful for date ranges
const monthStart = moment().startOf('month');
const monthEnd = moment().endOf('month');
console.log(`This month: ${monthStart.format('MMM D')} - ${monthEnd.format('MMM D')}`);
```

## Date Comparison and Query Divination

### Comparison Method Oracles

```javascript
const date1 = moment('2023-12-15');
const date2 = moment('2023-12-20');
const date3 = moment('2023-12-15');

// Basic comparisons
console.log(date1.isBefore(date2));        // true
console.log(date1.isAfter(date2));         // false
console.log(date1.isSame(date3));          // true
console.log(date1.isSameOrBefore(date2));  // true
console.log(date1.isSameOrAfter(date2));   // false

// Comparison with granularity
console.log(date1.isSame(date2, 'month')); // true (same month)
console.log(date1.isSame(date2, 'year'));  // true (same year)
console.log(date1.isSame(date2, 'day'));   // false (different day)

// Between dates
const testDate = moment('2023-12-17');
console.log(testDate.isBetween(date1, date2)); // true
console.log(testDate.isBetween(date1, date2, 'day', '[]')); // inclusive

// Difference between dates
const diff = date2.diff(date1);                    // milliseconds
const diffDays = date2.diff(date1, 'days');        // 5
const diffHours = date2.diff(date1, 'hours');      // 120
const diffExact = date2.diff(date1, 'days', true); // 5.0 (with decimals)

console.log('Difference in days:', diffDays);
console.log('Difference in hours:', diffHours);
```

### Date Query Enchantments

```javascript
const date = moment('2023-12-15T14:30:00');

// Get components
console.log('Year:', date.year());        // 2023
console.log('Month:', date.month());      // 11 (0-indexed)
console.log('Date:', date.date());        // 15
console.log('Day of week:', date.day());  // 5 (0=Sunday)
console.log('Hour:', date.hour());        // 14
console.log('Minute:', date.minute());    // 30

// Set components
const modified = date.clone()
    .year(2024)
    .month(0)  // January (0-indexed)
    .date(1)
    .hour(0)
    .minute(0)
    .second(0);

console.log('Modified:', modified.format());

// Day of year, week of year
console.log('Day of year:', date.dayOfYear());
console.log('Week of year:', date.week());

// Quarter
console.log('Quarter:', date.quarter());

// Leap year
console.log('Is leap year:', date.isLeapYear());

// Days in month
console.log('Days in month:', date.daysInMonth());
```

## Relative Time and Humanization Mysticism

### Relative Time Display Enchantments

```javascript
const now = moment();
const pastDates = [
    moment().subtract(30, 'seconds'),
    moment().subtract(5, 'minutes'),
    moment().subtract(2, 'hours'),
    moment().subtract(1, 'day'),
    moment().subtract(3, 'days'),
    moment().subtract(2, 'weeks'),
    moment().subtract(1, 'month'),
    moment().subtract(6, 'months'),
    moment().subtract(2, 'years')
];

const futureDates = [
    moment().add(30, 'seconds'),
    moment().add(5, 'minutes'),
    moment().add(2, 'hours'),
    moment().add(1, 'day'),
    moment().add(3, 'days'),
    moment().add(2, 'weeks'),
    moment().add(1, 'month'),
    moment().add(6, 'months'),
    moment().add(2, 'years')
];

console.log('Past dates:');
pastDates.forEach(date => {
    console.log(`${date.format('YYYY-MM-DD')} -> ${date.fromNow()}`);
});

console.log('\nFuture dates:');
futureDates.forEach(date => {
    console.log(`${date.format('YYYY-MM-DD')} -> ${date.fromNow()}`);
});

// Relative to specific date
const referenceDate = moment('2023-12-01');
const targetDate = moment('2023-12-15');
console.log('Relative to Dec 1:', targetDate.from(referenceDate)); // "in 14 days"

// Without suffix
console.log('Without suffix:', targetDate.fromNow(true)); // "2 weeks"
```

### Duration and Humanization Alchemy

```javascript
// Create durations
const duration1 = moment.duration(2, 'hours');
const duration2 = moment.duration(90, 'minutes');
const duration3 = moment.duration({
    hours: 2,
    minutes: 30,
    seconds: 45
});

// Humanize durations
console.log(duration1.humanize());              // "2 hours"
console.log(duration2.humanize());              // "an hour"
console.log(duration3.humanize());              // "3 hours"

// With suffix
console.log(duration1.humanize(true));          // "in 2 hours"

// Get duration components
console.log('Hours:', duration3.hours());       // 2
console.log('Minutes:', duration3.minutes());   // 30
console.log('Seconds:', duration3.seconds());   // 45
console.log('Total minutes:', duration3.asMinutes()); // 150.75

// Duration arithmetic
const total = moment.duration(1, 'hour').add(30, 'minutes');
console.log('Total duration:', total.humanize()); // "2 hours"

## Timezone Handling Mysticism

### Basic Timezone Operation Spells

```javascript
// Note: Requires moment-timezone plugin
// <script src="https://cdnjs.cloudflare.com/ajax/libs/moment-timezone/0.5.43/moment-timezone-with-data.min.js"></script>

// Create moment in specific timezone
const nyTime = moment.tz('2023-12-15 14:30', 'America/New_York');
const londonTime = moment.tz('2023-12-15 14:30', 'Europe/London');
const tokyoTime = moment.tz('2023-12-15 14:30', 'Asia/Tokyo');

console.log('NY Time:', nyTime.format('YYYY-MM-DD HH:mm z'));
console.log('London Time:', londonTime.format('YYYY-MM-DD HH:mm z'));
console.log('Tokyo Time:', tokyoTime.format('YYYY-MM-DD HH:mm z'));

// Convert between timezones
const utcTime = moment.utc('2023-12-15T14:30:00Z');
const convertedToNY = utcTime.clone().tz('America/New_York');
const convertedToLondon = utcTime.clone().tz('Europe/London');

console.log('UTC:', utcTime.format());
console.log('Converted to NY:', convertedToNY.format('YYYY-MM-DD HH:mm z'));
console.log('Converted to London:', convertedToLondon.format('YYYY-MM-DD HH:mm z'));

// Get timezone offset
console.log('NY Offset:', nyTime.utcOffset()); // minutes from UTC
console.log('London Offset:', londonTime.utcOffset());

// List available timezones
console.log('Available timezones:', moment.tz.names().slice(0, 10));
```

### Working with UTC Enchantments

```javascript
// Create UTC moment
const utcMoment = moment.utc();
const utcFromString = moment.utc('2023-12-15T14:30:00Z');

// Convert local to UTC
const localMoment = moment('2023-12-15T14:30:00');
const convertedToUTC = localMoment.utc();

// Convert UTC to local
const backToLocal = utcFromString.local();

console.log('UTC moment:', utcMoment.format());
console.log('Local to UTC:', convertedToUTC.format());
console.log('UTC to local:', backToLocal.format());

// Check if moment is UTC
console.log('Is UTC:', utcMoment.isUTC()); // true
console.log('Is UTC:', localMoment.isUTC()); // false
```

## Practical Legacy Manifestations

### Date Range Utility Enchantments

```javascript
class DateRangeHelper {
    static createRange(start, end, unit = 'day') {
        const startDate = moment(start);
        const endDate = moment(end);
        const dates = [];

        let current = startDate.clone();
        while (current.isSameOrBefore(endDate)) {
            dates.push(current.clone());
            current.add(1, unit);
        }

        return dates;
    }

    static getBusinessDays(start, end) {
        const range = this.createRange(start, end);
        return range.filter(date => {
            const dayOfWeek = date.day();
            return dayOfWeek !== 0 && dayOfWeek !== 6; // Not Sunday or Saturday
        });
    }

    static getWeekends(start, end) {
        const range = this.createRange(start, end);
        return range.filter(date => {
            const dayOfWeek = date.day();
            return dayOfWeek === 0 || dayOfWeek === 6; // Sunday or Saturday
        });
    }

    static getMonthBoundaries(year) {
        const months = [];
        for (let i = 0; i < 12; i++) {
            const start = moment({ year, month: i }).startOf('month');
            const end = moment({ year, month: i }).endOf('month');
            months.push({ month: i + 1, start, end });
        }
        return months;
    }
}

// Usage examples
const startDate = '2023-12-01';
const endDate = '2023-12-31';

const allDays = DateRangeHelper.createRange(startDate, endDate);
const businessDays = DateRangeHelper.getBusinessDays(startDate, endDate);
const weekends = DateRangeHelper.getWeekends(startDate, endDate);

console.log('Total days in December:', allDays.length);
console.log('Business days:', businessDays.length);
console.log('Weekend days:', weekends.length);

const monthBoundaries = DateRangeHelper.getMonthBoundaries(2023);
console.log('2023 months:', monthBoundaries.map(m =>
    `${m.month}: ${m.start.format('MMM D')} - ${m.end.format('MMM D')}`
));
```

### Event Scheduling System Rituals

```javascript
class EventScheduler {
    constructor() {
        this.events = [];
    }

    addEvent(title, startTime, duration, recurrence = null) {
        const event = {
            id: Date.now(),
            title,
            startTime: moment(startTime),
            endTime: moment(startTime).add(duration),
            recurrence
        };

        this.events.push(event);
        return event;
    }

    getEventsForDate(date) {
        const targetDate = moment(date);
        return this.events.filter(event => {
            // Check if event occurs on target date
            if (event.startTime.isSame(targetDate, 'day')) {
                return true;
            }

            // Check recurring events
            if (event.recurrence) {
                return this.isRecurringEventOnDate(event, targetDate);
            }

            return false;
        });
    }

    isRecurringEventOnDate(event, targetDate) {
        const { type, interval = 1, until } = event.recurrence;

        if (until && targetDate.isAfter(until)) {
            return false;
        }

        const daysDiff = targetDate.diff(event.startTime, 'days');

        switch (type) {
            case 'daily':
                return daysDiff >= 0 && daysDiff % interval === 0;
            case 'weekly':
                return daysDiff >= 0 && daysDiff % (7 * interval) === 0;
            case 'monthly':
                return targetDate.date() === event.startTime.date() &&
                       targetDate.diff(event.startTime, 'months') % interval === 0;
            default:
                return false;
        }
    }

    getUpcomingEvents(days = 7) {
        const today = moment().startOf('day');
        const endDate = moment().add(days, 'days').endOf('day');
        const upcoming = [];

        let current = today.clone();
        while (current.isSameOrBefore(endDate)) {
            const dayEvents = this.getEventsForDate(current);
            dayEvents.forEach(event => {
                upcoming.push({
                    ...event,
                    date: current.format('YYYY-MM-DD'),
                    timeUntil: event.startTime.fromNow()
                });
            });
            current.add(1, 'day');
        }

        return upcoming.sort((a, b) => a.startTime.diff(b.startTime));
    }

    findAvailableSlots(date, duration, workingHours = { start: 9, end: 17 }) {
        const targetDate = moment(date).startOf('day');
        const dayEvents = this.getEventsForDate(targetDate)
            .sort((a, b) => a.startTime.diff(b.startTime));

        const slots = [];
        const workStart = targetDate.clone().hour(workingHours.start).minute(0);
        const workEnd = targetDate.clone().hour(workingHours.end).minute(0);

        let currentTime = workStart.clone();

        for (const event of dayEvents) {
            if (event.startTime.diff(currentTime, 'minutes') >= duration) {
                slots.push({
                    start: currentTime.clone(),
                    end: event.startTime.clone(),
                    duration: event.startTime.diff(currentTime, 'minutes')
                });
            }
            currentTime = moment.max(currentTime, event.endTime);
        }

        // Check remaining time after last event
        if (workEnd.diff(currentTime, 'minutes') >= duration) {
            slots.push({
                start: currentTime.clone(),
                end: workEnd.clone(),
                duration: workEnd.diff(currentTime, 'minutes')
            });
        }

        return slots;
    }
}

// Usage example
const scheduler = new EventScheduler();

// Add events
scheduler.addEvent('Team Meeting', '2023-12-15 10:00', moment.duration(1, 'hour'), {
    type: 'weekly',
    interval: 1,
    until: moment('2024-03-15')
});

scheduler.addEvent('Lunch Break', '2023-12-15 12:00', moment.duration(1, 'hour'), {
    type: 'daily',
    interval: 1
});

// Get events for specific date
const todayEvents = scheduler.getEventsForDate('2023-12-15');
console.log('Today\'s events:', todayEvents.map(e => e.title));

// Find available slots
const availableSlots = scheduler.findAvailableSlots('2023-12-15', 60); // 60 minutes
console.log('Available 1-hour slots:', availableSlots.map(slot =>
    `${slot.start.format('HH:mm')} - ${slot.end.format('HH:mm')}`
));
```

### Age Calculator Divination

```javascript
class AgeCalculator {
    static calculateAge(birthDate, referenceDate = moment()) {
        const birth = moment(birthDate);
        const reference = moment(referenceDate);

        if (!birth.isValid() || !reference.isValid()) {
            throw new Error('Invalid date provided');
        }

        if (birth.isAfter(reference)) {
            throw new Error('Birth date cannot be in the future');
        }

        const years = reference.diff(birth, 'years');
        const months = reference.diff(birth.clone().add(years, 'years'), 'months');
        const days = reference.diff(birth.clone().add(years, 'years').add(months, 'months'), 'days');

        return {
            years,
            months,
            days,
            totalDays: reference.diff(birth, 'days'),
            totalMonths: reference.diff(birth, 'months'),
            formatted: `${years} years, ${months} months, ${days} days`
        };
    }

    static getNextBirthday(birthDate, referenceDate = moment()) {
        const birth = moment(birthDate);
        const reference = moment(referenceDate);

        let nextBirthday = birth.clone().year(reference.year());

        if (nextBirthday.isBefore(reference, 'day')) {
            nextBirthday.add(1, 'year');
        }

        const daysUntil = nextBirthday.diff(reference, 'days');
        const age = nextBirthday.year() - birth.year();

        return {
            date: nextBirthday,
            daysUntil,
            age,
            formatted: nextBirthday.format('MMMM Do, YYYY'),
            timeUntil: nextBirthday.fromNow()
        };
    }

    static isLeapYearBaby(birthDate) {
        const birth = moment(birthDate);
        return birth.month() === 1 && birth.date() === 29; // February 29
    }

    static getZodiacSign(birthDate) {
        const birth = moment(birthDate);
        const month = birth.month() + 1; // 1-indexed
        const day = birth.date();

        const signs = [
            { name: 'Capricorn', start: [12, 22], end: [1, 19] },
            { name: 'Aquarius', start: [1, 20], end: [2, 18] },
            { name: 'Pisces', start: [2, 19], end: [3, 20] },
            { name: 'Aries', start: [3, 21], end: [4, 19] },
            { name: 'Taurus', start: [4, 20], end: [5, 20] },
            { name: 'Gemini', start: [5, 21], end: [6, 20] },
            { name: 'Cancer', start: [6, 21], end: [7, 22] },
            { name: 'Leo', start: [7, 23], end: [8, 22] },
            { name: 'Virgo', start: [8, 23], end: [9, 22] },
            { name: 'Libra', start: [9, 23], end: [10, 22] },
            { name: 'Scorpio', start: [10, 23], end: [11, 21] },
            { name: 'Sagittarius', start: [11, 22], end: [12, 21] }
        ];

        for (const sign of signs) {
            const [startMonth, startDay] = sign.start;
            const [endMonth, endDay] = sign.end;

            if ((month === startMonth && day >= startDay) ||
                (month === endMonth && day <= endDay)) {
                return sign.name;
            }
        }

        return 'Unknown';
    }
}

// Usage examples
const birthDate = '1990-06-15';
const age = AgeCalculator.calculateAge(birthDate);
console.log('Age:', age.formatted);

const nextBirthday = AgeCalculator.getNextBirthday(birthDate);
console.log('Next birthday:', nextBirthday.formatted);
console.log('Days until birthday:', nextBirthday.daysUntil);

const zodiacSign = AgeCalculator.getZodiacSign(birthDate);
console.log('Zodiac sign:', zodiacSign);

const isLeapBaby = AgeCalculator.isLeapYearBaby('1992-02-29');
console.log('Is leap year baby:', isLeapBaby);
```

## Migration Guide to Modern Temporal Magic

### Why Migrate from Moment.js Legacy Spells?

1. **Bundle Size**: Moment.js is large (~67KB minified)
2. **Immutability**: Moment objects are mutable, causing bugs
3. **Tree Shaking**: Cannot tree-shake unused functionality
4. **Maintenance**: Project is in maintenance mode

### Migration to date-fns Temporal Sorcery

```javascript
// Moment.js
import moment from 'moment';

const now = moment();
const formatted = now.format('YYYY-MM-DD');
const tomorrow = moment().add(1, 'day');
const isAfter = moment('2023-12-20').isAfter('2023-12-15');

// date-fns equivalent
import { format, addDays, isAfter as isAfterFns } from 'date-fns';

const now = new Date();
const formatted = format(now, 'yyyy-MM-dd');
const tomorrow = addDays(new Date(), 1);
const isAfter = isAfterFns(new Date('2023-12-20'), new Date('2023-12-15'));
```

### Migration to Day.js Lightweight Magic

```javascript
// Moment.js
import moment from 'moment';

const now = moment();
const formatted = now.format('YYYY-MM-DD');
const tomorrow = moment().add(1, 'day');

// Day.js (similar API)
import dayjs from 'dayjs';

const now = dayjs();
const formatted = now.format('YYYY-MM-DD');
const tomorrow = dayjs().add(1, 'day');
```

### Migration to Luxon Advanced Enchantments

```javascript
// Moment.js
import moment from 'moment';

const now = moment();
const formatted = now.format('YYYY-MM-DD');
const tomorrow = moment().plus({ days: 1 });

// Luxon
import { DateTime } from 'luxon';

const now = DateTime.now();
const formatted = now.toFormat('yyyy-MM-dd');
const tomorrow = now.plus({ days: 1 });
```

## Wisdom of the Legacy Ancients (Legacy Code)

### Immutability Concern Protective Spells

```javascript
// ❌ Bad: Mutating moment objects
const date = moment('2023-12-15');
date.add(1, 'day'); // Mutates original
console.log(date.format()); // Now 2023-12-16

// ✅ Good: Clone before mutating
const date = moment('2023-12-15');
const tomorrow = date.clone().add(1, 'day');
console.log(date.format());     // Still 2023-12-15
console.log(tomorrow.format()); // 2023-12-16

// ✅ Better: Use immutable alternatives
const date = moment('2023-12-15');
const tomorrow = moment(date).add(1, 'day'); // Creates new instance
```

### Performance Optimization Enchantments

```javascript
// ❌ Bad: Creating moment objects in loops
const dates = [];
for (let i = 0; i < 1000; i++) {
    dates.push(moment().add(i, 'days').format('YYYY-MM-DD'));
}

// ✅ Good: Reuse base moment object
const baseDate = moment();
const dates = [];
for (let i = 0; i < 1000; i++) {
    dates.push(baseDate.clone().add(i, 'days').format('YYYY-MM-DD'));
}

// ✅ Better: Use native Date for simple operations
const baseDate = new Date();
const dates = [];
for (let i = 0; i < 1000; i++) {
    const date = new Date(baseDate);
    date.setDate(date.getDate() + i);
    dates.push(date.toISOString().split('T')[0]);
}
```

### Validation and Error Handling Protective Rituals

```javascript
// Always validate moment objects
function formatDate(input, format = 'YYYY-MM-DD') {
    const date = moment(input);

    if (!date.isValid()) {
        throw new Error(`Invalid date: ${input}`);
    }

    return date.format(format);
}

// Safe date operations
function safeDateOperation(input, operation) {
    try {
        const date = moment(input);
        if (!date.isValid()) {
            return null;
        }
        return operation(date);
    } catch (error) {
        console.error('Date operation failed:', error);
        return null;
    }
}

// Usage
const result = safeDateOperation('2023-12-15', date =>
    date.add(1, 'month').format('MMMM YYYY')
);
```

## Common Legacy Curses & Their Remedies

### Curse 1: Mutability Problem Corruption
Moment objects are mutable, which can cause unexpected behavior.

**Solution:**
```javascript
// Always clone when you need to preserve the original
const original = moment('2023-12-15');
const modified = original.clone().add(1, 'day');
```

### Curse 2: Month Indexing Confusion Hex
Months are 0-indexed in Moment.js (like JavaScript Date).

**Solution:**
```javascript
// December is month 11, not 12
const december = moment({ year: 2023, month: 11, day: 15 });
console.log(december.format('MMMM')); // "December"
```

### Curse 3: Timezone Confusion Malfunction
Working with timezones without moment-timezone plugin.

**Solution:**
```javascript
// Use moment-timezone for proper timezone handling
const nyTime = moment.tz('2023-12-15 14:30', 'America/New_York');
const utcTime = nyTime.utc();
```

### Curse 4: Bundle Size Impact Burden
Including entire Moment.js library increases bundle size significantly.

**Solution:**
```javascript
// Consider migrating to smaller alternatives
// Or use webpack plugins to reduce bundle size
// But best solution is to migrate to modern alternatives
```

## Sacred Texts & Mystical Sources

{% embed url="https://momentjs.com/" %}

{% embed url="https://momentjs.com/docs/" %}

{% embed url="https://momentjs.com/guides/#/-project-status/" %}

**⚠️ Migration Resources:**
- [You Don't Need Moment.js](https://github.com/you-dont-need/You-Dont-Need-Momentjs)
- [date-fns](https://date-fns.org/)
- [Day.js](https://day.js.org/)
- [Luxon](https://moment.github.io/luxon/)
```
