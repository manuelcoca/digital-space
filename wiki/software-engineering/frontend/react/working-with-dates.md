## How JavaScript Dates Work

-   JavaScript's `Date` object internally stores time as milliseconds since January 1, 1970, 00:00:00 UTC
-   Dates have no inherent timezone information stored within them
-   When displayed, dates default to the local timezone of the environment
-   UTC (Coordinated Universal Time) is the standard reference timezone for date operations
-   JavaScript provides methods for both UTC operations (`getUTCHours()`) and local time operations (`getHours()`)

## Frontend Date Handling

-   **Creating dates:**
    -   `new Date()` - current date/time in local timezone
    -   `new Date(isoString)` - parse ISO format string
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

-   **Always store dates in:**
    -   UTC timezone
    -   ISO 8601 format or timestamps (milliseconds)
    -   Database-native date types when available
-   **When receiving dates from frontend:**
    -   Validate format and values
    -   Convert to UTC if not already
    -   Use a consistent parsing strategy

## Frontend-Backend Communication

-   **Frontend to Backend:**
    -   Always send dates as ISO strings (`date.toISOString()`) or timestamps (`date.getTime()`)
    -   Never send local date string format (`date.toString()`)
    -   Include timezone information if it's relevant to the business logic
-   **Backend to Frontend:**
    -   Send dates in ISO 8601 format (with Z suffix for UTC) or as timestamps
    -   Be consistent across all API endpoints
    -   Document date format in API specs
-   **Best format choice:**
    -   ISO strings: human-readable, self-documenting, standard in REST APIs
    -   Timestamps: compact, unambiguous, better for calculations

## Libraries for Advanced Timezone Handling

-   **Recommended modern libraries:**
    -   Luxon - successor to Moment.js
    -   date-fns-tz - extension for date-fns
    -   Day.js with timezone plugin - lightweight alternative
-   **Other maintained libraries:**
    -   js-joda/timezone
    -   moment-timezone (legacy but still maintained)

## Common Pitfalls to Avoid

-   ❌ Manually adding/subtracting hours without considering DST (Daylight Saving Time)
-   ❌ Assuming all dates are in the same timezone across your application
-   ❌ Storing dates without timezone information when it matters for business logic
-   ❌ Parsing dates from strings without explicit formats
-   ❌ Using `Date` methods inconsistently (mixing UTC and local time methods)
-   ❌ Relying on implicit type conversion for dates
-   ❌ Using discontinued libraries like WallTime-js or TimeZoneJS
-   ❌ String manipulation for date operations instead of proper Date methods

## Testing Considerations

-   Test with users in different timezones
-   Test during DST transitions
-   Mock system time when testing date-dependent features
-   Include edge cases (leap years, month boundaries)
-   Test with both future and past dates

## Database Considerations

-   Different databases handle dates differently
-   Store in UTC or with explicit timezone information
-   Be aware of database-specific date formatting requirements
-   Consider database timezone settings

## Performance Tips

-   Avoid creating unnecessary Date objects in loops
-   Cache formatted date strings when displaying many dates
-   Consider using timestamps for intensive date calculations
-   Use ISO strings for better human debugging experience
