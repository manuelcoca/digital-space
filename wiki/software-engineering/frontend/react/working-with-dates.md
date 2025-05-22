## Frontend Date Handling

-   **JavaScript date object**
    -   JavaScript's `Date` object internally stores time as milliseconds since January 1, 1970, 00:00:00 UTC
    -   No inherent timezone information stored within them
    -   When displayed, dates default to the local timezone of the environment
-   **Creating dates:**
    -   `new Date()` - current date/time in local timezone
    -   `new Date(timestamp)` - create from milliseconds since epoch
    -   `new Date(year, month, day, hours, minutes, seconds)` - specific date/time (caution: month is 0-indexed)
-   **Formatting dates for display:**
    -   `toLocaleString()` - format according to user's locale
    -   `Intl.DateTimeFormat()` - more control over formatting
    -   `toISOString()` - UTC time in ISO 8601 format
-   **Timezone-specific display:**
    -   Use `date.toLocaleString('en-US', {timeZone: 'America/New_York'})` for specific timezone display
    -   Timezone identifiers follow IANA timezone database format (e.g., 'Europe/London', 'Asia/Tokyo')

## Backend Date Handling

-   **Backend processing:**
    -   Always calculates and saves dates as UTC - Users local time matters
    -   ISO 8601 format or timestamps (milliseconds)
    -   Database-native date types when available, usually DateTimeOffset is a good fit
-   **API communication**:
    -   ISO strings (`date.toISOString()`) or timestamps (`date.getTime()`) are more reliable than e.g framework specific data types
        -   ISO strings: human-readable, self-documenting, standard in REST APIs
        -   Timestamps: compact, unambiguous, better for calculations
    -   Consider sending local timezone to backend, as sometimes local timezone calculations are necessary
    -   Ideally, backend can handle both, utc and local strings to be send

## Examples to avoid

❌ Manually adding/subtracting hours without considering DST:

```javascript
// WRONG: Doesn't account for DST transitions
const tomorrow = new Date();
tomorrow.setHours(tomorrow.getHours() + 24);

// BETTER: Use timestamp arithmetic
const tomorrow = new Date(Date.now() + 24 * 60 * 60 * 1000);
```

❌ Storing dates without timezone information:

```javascript
// WRONG: Saving only date parts
const savedDate = {
	year: date.getFullYear(),
	month: date.getMonth(),
	day: date.getDate(),
};

// BETTER: Save ISO string or timestamp
const savedDate = date.toISOString();
// OR
const savedDate = date.getTime();
```

❌ Parsing dates from strings without explicit formats:

```javascript
// WRONG: Format is ambiguous and locale-dependent
const date = new Date("03/04/2023"); // Is this March 4 or April 3?

// BETTER: Use unambiguous ISO format
const date = new Date("2023-03-04T00:00:00Z");
```

❌ String manipulation for date operations:

```javascript
// WRONG: Direct string manipulation
const dateStr = "2023-05-20";
const nextDay = dateStr.split("-");
nextDay[2] = String(Number(nextDay[2]) + 1).padStart(2, "0");
const nextDayStr = nextDay.join("-");

// BETTER: Use Date methods
const date = new Date("2023-05-20T00:00:00Z");
date.setDate(date.getDate() + 1);
const nextDayStr = date.toISOString().split("T")[0];
```

❌ Incorrect date comparisons:

```javascript
// WRONG: Comparing date objects directly
if (date1 === date2) {
	/* never true for different instances */
}

// BETTER: Compare timestamps
if (date1.getTime() === date2.getTime()) {
	/* correct comparison */
}
```

❌ Not handling invalid dates:

```javascript
// WRONG: Assuming valid date
const date = new Date("not a date");
doSomethingWith(date); // Will use Invalid Date

// BETTER: Validate before use
const date = new Date("not a date");
if (isNaN(date.getTime())) {
	// Handle invalid date
} else {
	doSomethingWith(date);
}
```

❌ Assuming new Date() uses UTC:

```javascript
// WRONG: Assuming new Date() uses UTC
const now = new Date();
console.log(now.getHours()); // Shows local hours, not UTC

// BETTER: Be explicit about UTC
const nowUtc = new Date();
console.log(nowUtc.getUTCHours()); // UTC hours
```

❌ Parsing month incorrectly:

```javascript
// WRONG: Forgetting months are 0-indexed
const date = new Date(2023, 12, 15); // Creates January 15, 2024!

// BETTER: Remember months are 0-indexed (0-11)
const date = new Date(2023, 11, 15); // December 15, 2023
```
