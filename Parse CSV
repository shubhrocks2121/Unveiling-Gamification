# Data Capture and Manipulation in `parseAndStoreCsv` Function

The `parseAndStoreCsv` function is an asynchronous function that reads a CSV file, processes its data, and stores it in a database. Below is a detailed explanation of how the data is captured and manipulated.

---

## **Function Overview**
- **Input**: 
  - `filePath`: Path to the CSV file.
  - `userId`: ID of the user uploading the file.
- **Output**: 
  - A promise that resolves to an object with `success` (boolean) and `message` (string) indicating the result of the operation.

---

## **Data Capture and Manipulation**

### **1. Data Structures**
- **`records` Array**:
  - Stores processed rows from the CSV file.
  - Each record contains fields like `uid`, `name`, `order_id`, `order_status`, `timestamp_created`, `timestamp_updated`, `domain`, `buyer_app_id`, `total_price`, and `uploaded_by`.

- **`recordMap` Map**:
  - Tracks unique `order_id` values and their associated `order_status` and `buyerAppId`.
  - Prevents duplicate orders with the same `order_id` and `order_status`.

---

### **2. CSV Parsing**
- The CSV file is read using `fs.createReadStream` and piped into a CSV parser.
- The parser processes each row using the `on("data")` event.

---

### **3. Row Processing**
For each row in the CSV file:
1. **Row Count Check**:
   - If the number of rows exceeds 100,000, the function rejects with an error.

2. **Normalization**:
   - Row keys are normalized (trimmed, converted to lowercase, and spaces replaced with underscores).
   - Empty or invalid fields are logged, and the function rejects if any required field is missing.

3. **Validation**:
   - Checks for missing or unexpected fields.
   - Ensures all required fields (`phone_number`, `name`, `order_id`, `order_status`, `timestamp_created`, `domain`, `total_price`) are present.

4. **Duplicate Order Check**:
   - If an `order_id` already exists in `recordMap` with the same `order_status` and `buyerAppId`, the function rejects with a duplicate error.

5. **Timestamp Parsing**:
   - The `timestamp_created` field is parsed using `moment-timezone` and converted to a `Date` object.
   - Invalid timestamps result in rejection.

6. **Data Transformation**:
   - Each row is transformed into a structured object and added to the `records` array.
   - The `recordMap` is updated with the `order_id`, `order_status`, and `buyerAppId`.

---

### **4. Data Storage**
After processing all rows:
1. **Categorization**:
   - Records are categorized into `newOrders` and `cancellations` based on `order_status`.

2. **Database Operations**:
   - `newOrders` and `cancellations` are processed and inserted into the database using helper functions (`processNewOrders`, `processCancellations`, and `bulkInsertDataIntoDb`).

3. **Cleanup**:
   - The CSV file is deleted using `fs.unlinkSync`.
   - The database connection is closed using `prisma.$disconnect`.

---

### **5. Error Handling**
- Errors during CSV parsing, row processing, or database operations are caught and logged.
- The function rejects with an appropriate error message.

---

## **Key Features**
- **Scalability**: Limits the number of rows to 100,000 to prevent memory overload.
- **Data Integrity**: Ensures all required fields are present and valid.
- **Duplicate Prevention**: Uses a `Map` to track and prevent duplicate orders.
- **Asynchronous Processing**: Handles large files efficiently using streams and promises.

---

## **Example Output**
- **Success**: 
  ```json
  { "success": true, "message": "CSV data stored successfully" }
