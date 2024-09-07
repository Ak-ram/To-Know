1. **`BmToDateOrDateString`**
   - **Purpose:** Converts a date string into a `Date` object or a string representation of the date based on the `returnDate` flag.
   - **Parameters:**
     - `value`: The date string to convert.
     - `returnDate`: If `true`, returns a `Date` object; otherwise, returns a string. Defaults to `false`.
   - **Returns:** A `Date` object or a string depending on `returnDate`.

   ```typescript
   // Example usage:
   const dateObj = BmToDateOrDateString('2023-09-06', true); // Date object
   const dateString = BmToDateOrDateString('06/09/2023'); // String representation
   ```

2. **`BmFormateStringDate`**
   - **Purpose:** Formats a date string into a consistent format (YYYY/MM/DD or similar).
   - **Parameters:**
     - `value`: The date string to format.
   - **Returns:** Formatted date string.

   ```typescript
   // Example usage:
   const formattedDate = BmFormateStringDate('06/09/2023'); // '2023/09/06'
   ```
