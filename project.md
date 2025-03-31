### **Emailothar: Program Description**
Emailothar is a sleek, user-friendly application designed to streamline the process of extracting and distributing specific pages from PDF documents to employees via email, ensuring efficient communication and effortless document management.

---

### **Functional Requirements**

#### **1. Data Input**
1.1. **CSV File**:
   - Upon initialization, the program creates an empty CSV file that can be populated through the graphical interface by entering `id`, `email`, and `note`.
   - The CSV file supports both commas (`,`) and semicolons (`;`) as delimiters.
   - If the user uploads an incorrectly formatted CSV (e.g., with extra spaces or missing quotes), the program will automatically correct the format.
   - Exporting the CSV file after editing is not required.

1.2. **PDF File**:
   - The program does **not** support encrypted PDF files (password-protected).
   - If a PDF contains mixed content (text + images), the program will search for `id` matches on the page and send the original page (with images) to the corresponding email address.

---

#### **2. PDF Processing**
2.1. **Partial ID Matches**:
   - Partial matches are considered valid. For example, if `id="abc"` is found in the string `"abc123"`, it will be treated as a match.
   - The program ignores case sensitivity for both text search and `id` values in the CSV.

2.2. **Pages Without Text**:
   - If a page contains only images, it is considered empty. No OCR processing is performed, and such pages are logged as containing no textual content.

---

#### **3. Sending Messages**
3.1. **Gmail API**:
   - The program uses Gmail API to send emails.
   - The expected volume of emails per run is approximately 50.
   - There are no business-imposed restrictions on attachment sizes.

3.2. **Send Queue**:
   - If an email fails to send after 3 attempts, the corresponding file will be renamed with the prefix `[failed to send]` for manual processing.
   - The program will notify the user about such failed emails via a banner notification: `[failed to send] <filename>`.

---

#### **4. Logging**
4.1. **Log Format**:
   - Logs will use a simple format.
   - Logs are not accessible through the GUI and are stored only on the host machine.

4.2. **Log Rotation**:
   - Old logs are automatically deleted without requiring user consent.

---

#### **5. File Storage**
5.1. **Output Folder**:
   - The `output` folder resides on the host machine.
   - Backup of files in the `output` folder is not required.

5.2. **Automatic Cleanup**:
   - Files older than 365 days are automatically deleted when the program starts.

---

#### **6. User Interface**
6.1. **GUI**:
   - The GUI is implemented using **PyQt**.
   - Only a black, white, and purple color scheme is supported (no dark mode).

6.2. **Stop Button**:
   - Pressing the "Stop" button terminates the current task.

---

### **Non-Functional Requirements**

#### **1. Google API Integration**
1.1. The program uses Gmail API for sending emails.
1.2. Authorization is performed via OAuth 2.0 with minimal user interaction ("one-click").
1.3. Credentials are stored in Docker secrets and must be encrypted.

#### **2. Docker**
2.1. The program must support running both inside and outside a Docker container (for local testing).
2.2. Ports:
   - No specific ports need to be opened in the container unless required for logging or Google API interaction. Standard ports for Gmail API (HTTPS) are sufficient.

#### **3. Dependencies**
3.1. The number of dependencies must be minimized to reduce the container size.
3.2. Library versions should be compatible with each other and aligned with the latest updates available in the knowledge base of a neural network that is working on this code.

#### **4. Security**
4.1. Logs may contain confidential information but are stored securely on the user's local machine. Access is restricted to the user.