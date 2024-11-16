# Complete JavaScript Date Methods Guide

## 1. Date Object Creation Methods

| Method | Description | Example |
|--------|-------------|----------|
| `new Date()` | Current date/time | `const now = new Date()` |
| `new Date(milliseconds)` | From milliseconds since 1970 | `const past = new Date(1000000000000)` |
| `new Date(dateString)` | From date string | `const date = new Date("2024-11-16")` |
| `new Date(year, month, day, hours, minutes, seconds, ms)` | From components | `const future = new Date(2024, 10, 16, 14, 30, 0)` |

### Creation Examples:

```javascript
// Example 1: Different ways to create current date
const currentDate1 = new Date();
const currentDate2 = new Date(Date.now());
const currentDate3 = new Date(new Date().toISOString());

console.log("Current date variations:");
console.log(currentDate1);  // 2024-11-16T14:30:00.000Z
console.log(currentDate2);  // 2024-11-16T14:30:00.000Z
console.log(currentDate3);  // 2024-11-16T14:30:00.000Z

// Example 2: Creating dates from strings
const date1 = new Date("2024-11-16");           // ISO format
const date2 = new Date("11/16/2024");           // US format
const date3 = new Date("16 November 2024");     // Long format
const date4 = new Date("2024-11-16T14:30:00Z"); // ISO with time

console.log("Dates from strings:");
console.log(date1.toLocaleString());
console.log(date2.toLocaleString());
console.log(date3.toLocaleString());
console.log(date4.toLocaleString());

// Example 3: Creating future/past dates
const tomorrow = new Date(Date.now() + 86400000);  // Add 24 hours in milliseconds
const nextWeek = new Date(Date.now() + 7 * 86400000);
const lastYear = new Date(new Date().setFullYear(new Date().getFullYear() - 1));

console.log("Relative dates:");
console.log(`Tomorrow: ${tomorrow.toDateString()}`);
console.log(`Next week: ${nextWeek.toDateString()}`);
console.log(`Last year: ${lastYear.toDateString()}`);
```

## 2. Get Methods Reference

| Method | Returns | Example |
|--------|---------|----------|
| `getFullYear()` | 4-digit year | `date.getFullYear()` // 2024 |
| `getMonth()` | Month (0-11) | `date.getMonth()` // 10 (November) |
| `getDate()` | Day (1-31) | `date.getDate()` // 16 |
| `getDay()` | Weekday (0-6) | `date.getDay()` // 6 (Saturday) |
| `getHours()` | Hours (0-23) | `date.getHours()` // 14 |
| `getMinutes()` | Minutes (0-59) | `date.getMinutes()` // 30 |
| `getSeconds()` | Seconds (0-59) | `date.getSeconds()` // 45 |
| `getMilliseconds()` | Milliseconds (0-999) | `date.getMilliseconds()` // 500 |
| `getTime()` | Milliseconds since 1970 | `date.getTime()` // 1737045600000 |
| `getTimezoneOffset()` | Minutes from UTC | `date.getTimezoneOffset()` // -300 |

### Get Methods Examples:

```javascript
// Example 1: Getting date components for scheduling
function getAppointmentDetails(dateStr) {
    const appointment = new Date(dateStr);
    const days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];
    const months = ['January', 'February', 'March', 'April', 'May', 'June', 
                   'July', 'August', 'September', 'October', 'November', 'December'];
    
    return {
        dayName: days[appointment.getDay()],
        monthName: months[appointment.getMonth()],
        date: appointment.getDate(),
        year: appointment.getFullYear(),
        time: `${appointment.getHours()}:${appointment.getMinutes()}`
    };
}

const appointment = getAppointmentDetails("2024-11-16T14:30:00");
console.log("Appointment Details:", appointment);

// Example 2: Checking business hours
function isBusinessHours(date) {
    const hours = date.getHours();
    const day = date.getDay();
    const isWeekday = day > 0 && day < 6;
    const isWorkHours = hours >= 9 && hours < 17;
    
    return {
        isOpen: isWeekday && isWorkHours,
        currentHour: hours,
        isWeekday: isWeekday,
        dayOfWeek: day
    };
}

console.log("Business Hours Check:", isBusinessHours(new Date()));

// Example 3: Creating a digital clock
function getDigitalTime(date) {
    const hours = date.getHours().toString().padStart(2, '0');
    const minutes = date.getMinutes().toString().padStart(2, '0');
    const seconds = date.getSeconds().toString().padStart(2, '0');
    const milliseconds = date.getMilliseconds().toString().padStart(3, '0');
    
    return {
        time: `${hours}:${minutes}:${seconds}`,
        precise: `${hours}:${minutes}:${seconds}.${milliseconds}`,
        hour12: date.toLocaleTimeString()
    };
}

console.log("Digital Time:", getDigitalTime(new Date()));
```

## 3. Set Methods Reference

| Method | Description | Example |
|--------|-------------|----------|
| `setFullYear()` | Sets year | `date.setFullYear(2025)` |
| `setMonth()` | Sets month (0-11) | `date.setMonth(11)` |
| `setDate()` | Sets day (1-31) | `date.setDate(25)` |
| `setHours()` | Sets hours (0-23) | `date.setHours(14)` |
| `setMinutes()` | Sets minutes (0-59) | `date.setMinutes(30)` |
| `setSeconds()` | Sets seconds (0-59) | `date.setSeconds(45)` |
| `setMilliseconds()` | Sets milliseconds (0-999) | `date.setMilliseconds(500)` |
| `setTime()` | Sets milliseconds since 1970 | `date.setTime(1737045600000)` |

### Set Methods Examples:

```javascript
// Example 1: Scheduling future events
function scheduleEvent(baseDate, { days = 0, hours = 0, minutes = 0 }) {
    const eventDate = new Date(baseDate);
    
    if (days) eventDate.setDate(eventDate.getDate() + days);
    if (hours) eventDate.setHours(eventDate.getHours() + hours);
    if (minutes) eventDate.setMinutes(eventDate.getMinutes() + minutes);
    
    return eventDate;
}

const now = new Date();
console.log("Meeting in 2 days:", scheduleEvent(now, { days: 2 }));
console.log("Lunch in 3 hours:", scheduleEvent(now, { hours: 3 }));
console.log("Call in 45 minutes:", scheduleEvent(now, { minutes: 45 }));

// Example 2: Setting recurring events
function createRecurringEvent(startDate, { recurEvery, endAfter }) {
    const events = [];
    const start = new Date(startDate);
    
    for (let i = 0; i < endAfter; i++) {
        const eventDate = new Date(start);
        eventDate.setDate(start.getDate() + (i * recurEvery));
        events.push(eventDate);
    }
    
    return events;
}

const weeklyMeetings = createRecurringEvent("2024-11-16", { recurEvery: 7, endAfter: 4 });
console.log("Weekly Meetings Schedule:", weeklyMeetings);

// Example 3: Setting business deadline
function setBusinessDeadline(startDate, businessDays) {
    const deadline = new Date(startDate);
    let addedDays = 0;
    
    while (addedDays < businessDays) {
        deadline.setDate(deadline.getDate() + 1);
        if (deadline.getDay() !== 0 && deadline.getDay() !== 6) {
            addedDays++;
        }
    }
    
    deadline.setHours(17, 0, 0, 0); // Set to 5 PM
    return deadline;
}

console.log("Project Deadline:", setBusinessDeadline(new Date(), 10));
```

## 4. Formatting Methods Reference

| Method | Description | Example Output |
|--------|-------------|----------------|
| `toLocaleString()` | Locale format | "11/16/2024, 2:45:30 PM" |
| `toUTCString()` | UTC format | "Sat, 16 Nov 2024 14:45:30 GMT" |
| `toISOString()` | ISO format | "2024-11-16T14:45:30.500Z" |
| `toDateString()` | Date only | "Sat Nov 16 2024" |

### Formatting Examples:

```javascript
// Example 1: International date formatting
function formatDateMultiLanguage(date) {
    const options = {
        weekday: 'long',
        year: 'numeric',
        month: 'long',
        day: 'numeric',
        hour: '2-digit',
        minute: '2-digit'
    };
    
    return {
        us: date.toLocaleString('en-US', options),
        uk: date.toLocaleString('en-GB', options),
        germany: date.toLocaleString('de-DE', options),
        japan: date.toLocaleString('ja-JP', options),
        arabic: date.toLocaleString('ar-SA', options)
    };
}

console.log("Multi-language dates:", formatDateMultiLanguage(new Date()));

// Example 2: Custom date formatting
function formatDateCustom(date) {
    const days = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'];
    const months = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 
                   'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'];
    
    return {
        short: `${date.getDate()}/${date.getMonth() + 1}/${date.getFullYear()}`,
        medium: `${days[date.getDay()]}, ${months[date.getMonth()]} ${date.getDate()}`,
        long: date.toLocaleDateString(undefined, { 
            weekday: 'long',
            year: 'numeric',
            month: 'long',
            day: 'numeric'
        }),
        iso: date.toISOString()
    };
}

console.log("Custom Formats:", formatDateCustom(new Date()));

// Example 3: Relative time formatting
function getRelativeTime(date) {
    const now = new Date();
    const diff = date - now;
    const seconds = Math.floor(Math.abs(diff) / 1000);
    const minutes = Math.floor(seconds / 60);
    const hours = Math.floor(minutes / 60);
    const days = Math.floor(hours / 24);
    
    if (diff < 0) {
        if (seconds < 60) return 'just now';
        if (minutes < 60) return `${minutes} minutes ago`;
        if (hours < 24) return `${hours} hours ago`;
        if (days < 30) return `${days} days ago`;
        return date.toLocaleDateString();
    } else {
        if (seconds < 60) return 'in a few seconds';
        if (minutes < 60) return `in ${minutes} minutes`;
        if (hours < 24) return `in ${hours} hours`;
        if (days < 30) return `in ${days} days`;
        return date.toLocaleDateString();
    }
}

const futureDate = new Date(Date.now() + 3600000); // 1 hour from now
const pastDate = new Date(Date.now() - 7200000);   // 2 hours ago
console.log("Relative Times:");
console.log(getRelativeTime(futureDate));
console.log(getRelativeTime(pastDate));
```

## 5. Practical Time Calculations

```javascript
// Example 1: Countdown Timer
function createCountdown(targetDate) {
    const target = new Date(targetDate);
    
    return function getTimeRemaining() {
        const now = new Date();
        const diff = target - now;
        
        if (diff <= 0) return "Expired";
        
        const days = Math.floor(diff / (1000 * 60 * 60 * 24));
        const hours = Math.floor((diff % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
        const minutes = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60));
        const seconds = Math.floor((diff % (1000 * 60)) / 1000);
        
        return {
            total: diff,
            days,
            hours,
            minutes,
            seconds,
            formatted: `${days}d ${hours}h ${minutes}m ${seconds}s`
        };
    };
}

const countdown = createCountdown("2024-12-31T00:00:00");
console.log("New Year Countdown:", countdown());

// Example 2: Business Hours Calculator
function calculateBusinessHours(startDate, endDate) {
    const start = new Date(startDate);
    const end = new Date(endDate);
    let totalHours = 0;
    
    while (start < end) {
        if (start.getDay() !== 0 && start.getDay() !== 6) {
            const startHour = start.getHours();
            if (startHour >= 9 && startHour < 17) {
                totalHours++;
            }
        }
        start.setHours(start.getHours() + 1);
    }
    
    return {
        totalHours,
        workDays: Math.ceil(totalHours / 8),
        billableAmount: totalHours * 150 // Assuming $150/hour
    };
}

console.log("Business Hours:", calculateBusinessHours(
    "2024-11-16T09:00:00",
    "2024-11-20T17:00:00"
));

// Example 3: Date Range Iterator
function* dateRangeGenerator(startDate, endDate, { skipWeekends = false, skipHolidays = [] } = {}) {
    const current = new Date(startDate);
    const end = new Date(endDate);
    
    while (current <= end) {
        const isWeekend = current.getDay() === 0 || current.getDay() === 6;
        const isHoliday = skipHolidays.some(holiday => 
            holiday.getDate() === current.getDate() &&
            holiday.getMonth() === current.getMonth()
        );
        
        if ((!skipWeekends || !isWeekend) && (!isHoliday)) {
            yield new Date(current);
        }
        
        current.setDate(current.getDate() + 1);
    }
}

// Usage example
const holidays = [
    new Date("2024-12-25"), // Christmas
    new Date("2024-12-31")  // New Year's Eve
];

const dateRange = dateRangeGenerator(
    "2024-12-20",
    "2024-12-31",
    { skipWeekends: true, skipHolidays: holidays }
);

console.log("Working days in range:");
for (const date of dateRange) {
    console.log(date.toDateString());
}

// Example 4: Age Calculator with Extra Details
function calculateDetailedAge(birthDate) {
    const birth = new Date(birthDate);
    const now = new Date();
    
    const yearDiff = now.getFullYear() - birth.getFullYear();
    const monthDiff = now.getMonth() - birth.getMonth();
    const dayDiff = now.getDate() - birth.getDate();
    
    let ageYears = yearDiff;
    let ageMonths = monthDiff;
    let ageDays = dayDiff;
    
    if (dayDiff < 0) {
        ageMonths--;
        const lastMonth = new Date(now.getFullYear(), now.getMonth() - 1, birth.getDate());
        ageDays = Math.floor((now - lastMonth) / (1000 * 60 * 60 * 24));
    }
    
    if (monthDiff < 0 || (monthDiff === 0 && dayDiff < 0)) {
        ageYears--;
        ageMonths += 12;
    }
    
    const nextBirthday = new Date(now.getFullYear(), birth.getMonth(), birth.getDate());
    if (nextBirthday < now) {
        nextBirthday.setFullYear(nextBirthday.getFullYear() + 1);
    }
    
    const daysToNextBirthday = Math.ceil((nextBirthday - now) / (1000 * 60 * 60 * 24));
    
    return {
        years: ageYears,
        months: ageMonths,
        days: ageDays,
        totalMonths: ageYears * 12 + ageMonths,
        totalDays: Math.floor((now - birth) / (1000 * 60 * 60 * 24)),
        nextBirthday: {
            date: nextBirthday,
            daysUntil: daysToNextBirthday,
            dayOfWeek: nextBirthday.toLocaleDateString('en-US', { weekday: 'long' })
        },
        zodiacSign: getZodiacSign(birth),
        isLeapYearBirth: isLeapYear(birth.getFullYear())
    };
}

function getZodiacSign(date) {
    const month = date.getMonth() + 1;
    const day = date.getDate();
    
    if ((month === 3 && day >= 21) || (month === 4 && day <= 19)) return "Aries";
    if ((month === 4 && day >= 20) || (month === 5 && day <= 20)) return "Taurus";
    if ((month === 5 && day >= 21) || (month === 6 && day <= 20)) return "Gemini";
    if ((month === 6 && day >= 21) || (month === 7 && day <= 22)) return "Cancer";
    if ((month === 7 && day >= 23) || (month === 8 && day <= 22)) return "Leo";
    if ((month === 8 && day >= 23) || (month === 9 && day <= 22)) return "Virgo";
    if ((month === 9 && day >= 23) || (month === 10 && day <= 22)) return "Libra";
    if ((month === 10 && day >= 23) || (month === 11 && day <= 21)) return "Scorpio";
    if ((month === 11 && day >= 22) || (month === 12 && day <= 21)) return "Sagittarius";
    if ((month === 12 && day >= 22) || (month === 1 && day <= 19)) return "Capricorn";
    if ((month === 1 && day >= 20) || (month === 2 && day <= 18)) return "Aquarius";
    return "Pisces";
}

function isLeapYear(year) {
    return (year % 4 === 0 && year % 100 !== 0) || (year % 400 === 0);
}

console.log("Detailed Age:", calculateDetailedAge("1990-05-15"));

// Example 5: Smart Event Scheduler
function createEventScheduler(eventConfig) {
    return {
        generateSchedule(startDate, occurrences) {
            const schedule = [];
            let currentDate = new Date(startDate);
            
            while (schedule.length < occurrences) {
                if (this.isValidEventDay(currentDate)) {
                    schedule.push({
                        date: new Date(currentDate),
                        formattedDate: currentDate.toLocaleDateString(),
                        dayOfWeek: currentDate.toLocaleDateString('en-US', { weekday: 'long' }),
                        time: currentDate.toLocaleTimeString(),
                        type: this.getEventType(currentDate)
                    });
                }
                currentDate.setDate(currentDate.getDate() + 1);
            }
            
            return schedule;
        },
        
        isValidEventDay(date) {
            const dayOfWeek = date.getDay();
            const isWeekend = dayOfWeek === 0 || dayOfWeek === 6;
            
            if (eventConfig.weekdaysOnly && isWeekend) return false;
            if (eventConfig.excludeDates && this.isExcludedDate(date)) return false;
            if (eventConfig.timeRange && !this.isWithinTimeRange(date)) return false;
            
            return true;
        },
        
        isExcludedDate(date) {
            return eventConfig.excludeDates.some(excludedDate => 
                excludedDate.getDate() === date.getDate() &&
                excludedDate.getMonth() === date.getMonth() &&
                excludedDate.getFullYear() === date.getFullYear()
            );
        },
        
        isWithinTimeRange(date) {
            const hours = date.getHours();
            return hours >= eventConfig.timeRange.start && 
                   hours <= eventConfig.timeRange.end;
        },
        
        getEventType(date) {
            const hours = date.getHours();
            if (hours < 12) return 'Morning Session';
            if (hours < 17) return 'Afternoon Session';
            return 'Evening Session';
        }
    };
}

// Usage example
const eventConfig = {
    weekdaysOnly: true,
    timeRange: { start: 9, end: 17 },
    excludeDates: [
        new Date("2024-12-25"),
        new Date("2024-12-31")
    ]
};

const scheduler = createEventScheduler(eventConfig);
const eventSchedule = scheduler.generateSchedule(new Date(), 5);
console.log("Event Schedule:", eventSchedule);

// Example 6: Date Formatter with Templates
function createDateFormatter(templates = {}) {
    const defaultTemplates = {
        short: 'MM/DD/YYYY',
        medium: 'MMM DD, YYYY',
        long: 'MMMM DD, YYYY',
        full: 'dddd, MMMM DD, YYYY'
    };

    const formatTokens = {
        YYYY: date => date.getFullYear(),
        MM: date => String(date.getMonth() + 1).padStart(2, '0'),
        DD: date => String(date.getDate()).padStart(2, '0'),
        MMM: date => date.toLocaleString('en', { month: 'short' }),
        MMMM: date => date.toLocaleString('en', { month: 'long' }),
        dd: date => String(date.getDate()),
        dddd: date => date.toLocaleString('en', { weekday: 'long' }),
        HH: date => String(date.getHours()).padStart(2, '0'),
        mm: date => String(date.getMinutes()).padStart(2, '0'),
        ss: date => String(date.getSeconds()).padStart(2, '0')
    };

    return {
        format(date, templateName = 'medium') {
            const template = templates[templateName] || defaultTemplates[templateName] || templateName;
            
            return template.replace(/YYYY|MM|DD|MMM|MMMM|dd|dddd|HH|mm|ss/g, match => 
                formatTokens[match](date)
            );
        },
        
        formatRange(startDate, endDate, templateName = 'medium') {
            return `${this.format(startDate, templateName)} - ${this.format(endDate, templateName)}`;
        }
    };
}

const formatter = createDateFormatter({
    custom: 'dddd, MMM DD, YYYY at HH:mm',
    dateOnly: 'MM/DD/YYYY',
    timeOnly: 'HH:mm:ss'
});

const date = new Date();
console.log("Formatted Dates:");
console.log("Short:", formatter.format(date, 'short'));
console.log("Custom:", formatter.format(date, 'custom'));
console.log("Time Only:", formatter.format(date, 'timeOnly'));
```

These examples demonstrate advanced date manipulation and formatting techniques, including:
1. Date range generation with weekend and holiday exclusions
2. Detailed age calculation with zodiac sign and leap year information
3. Smart event scheduling system with configuration options
4. Custom date formatting system with templates

Each example includes practical use cases and can be extended or modified based on specific needs. Would you like me to explain an