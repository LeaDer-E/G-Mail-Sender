
# G-Mail_Sender

**Fast, SMTP-based bulk mailer for Sending Fast Mails.**  
*Tested on Gmail only (smtp.gmail.com:587). Use responsibly.*

---

## Important Warning
- **Do not send more than ~500 emails per day from a single Gmail account.** Exceeding this limit risks temporary blocks or account suspension. Use this script only for legitimate, non-spam purposes.

---

## What this script does
- Reads sender credentials from `signin.txt` (line 1: email, line 2: App Password). Spaces in the App Password are removed automatically.  
- Reads recipients from `Mails.txt` (one email per line).  
- Reads subject and body from `Message.txt` (line 1 = Subject, remaining lines = Body).  
- Collects **all files** inside the `Attachment/` folder (any file type) and attaches them to each email.  
- Verifies SMTP login **before** sending any email. If login fails the script exits.  
- Sends each recipient a separate email (one recipient per message).  
- Produces a colored, well-formatted terminal output showing connection status, per-email status, timestamps, and error info if any.  
- Creates a `Result/` folder and saves:
  - `Result.txt` — text log (timestamp | index | email | SENT/FAILED | info).  
  - `Result.xlsx` — Excel report with columns `No.`, `Mail`, `Status`, `Time`.

---

## Terminal output (colors & meanings)
- `🟡 [CONNECTED]` — Yellow: SMTP login successful (shown once before sending).  
- `🔵 [START]` — Blue: Sending loop started.  
- `🟢 [✓]` — Green: Email sent successfully. Shows index, recipient, status, timestamp, and number of attachments.  
- `🔴 [✗]` — Red: Sending failed. Shows index, recipient, timestamp, and the error message.

Example:
```
[✓]  1  recipient@example.com  Sent   2025-10-24 09:02:09  3 attach(s)
[✗]  2  bad@example.com       Failed 2025-10-24 09:02:15  SMTPAuthenticationError: ...
```

Summary shows colored counts for Sent and Failed.

---

## Result Excel (`Result/Result.xlsx`) — formatting details
- Columns:
  - **A** = `No.` (serial number)
  - **B** = `Mail`
  - **C** = `Status` (Sent / Failed)
  - **D** = `Time` (format `YYYY-MM-DD HH:MM:SS`)
- Visual formatting:
  - **Header row (row 1)**: orange background, italic, bold; frozen (freeze pane at B2).  
  - **First column (A)**: orange background, italic.  
  - **All cells** have thin borders.  
  - **Status cells**: green fill for `Sent`, red fill for `Failed`.

---

## Required files & folders
Place these in the same folder as the script:

- `signin.txt`  
  ```
  your.email@gmail.com
  abcd efgh ijkl mnop
  ```
  — Line 1: sender email. Line 2: App Password (spaces allowed; script strips them).

- `Mails.txt` — one recipient email per line.

- `Message.txt`  
  ```
  Email subject here
  Body line 1
  Body line 2
  ```

- `Attachment/` — a folder containing any files you want attached to all emails (any file types are included), Don't Forget to delete the .txt i leave it.

---

## Dependencies
Install required Python packages:
```bash
pip install colorama openpyxl
```
The script uses Python standard libraries: `smtplib`, `ssl`, `email`, `mimetypes`, `pathlib`, and `datetime`.

---

## SMTP providers & configuration
- **Gmail (default and tested)**  
  - Server: `smtp.gmail.com`  
  - Port: `587` (STARTTLS)  
  - Requires App Password if 2-Step Verification is enabled.

- **Outlook / Office365**  
  - Server: `smtp.office365.com` or `smtp-mail.outlook.com`  
  - Port: `587` (STARTTLS)

- **Yandex**  
  - Server: `smtp.yandex.com` or `smtp.yandex.ru`  
  - Port: `465` (SSL) or `587` (STARTTLS)

If your provider requires SSL on port 465, modify the script to use `smtplib.SMTP_SSL` instead of `starttls()` if you can.



---

## App Password — how to obtain (Gmail)
1. Enable **2-Step Verification** for your Google account.  
2. Go to **Security → App passwords**.  
3. Choose an app name (e.g. `PythonMailer`) and generate an App Password.  
4. Copy the 16-character password and paste it in `signin.txt` (the script strips spaces automatically).

> App passwords require 2-Step Verification. If `App passwords` is not available, your account may be managed by an organization that disabled the feature.

![G-Mail App Password](https://github.com/user-attachments/assets/1f79e272-97ef-46b0-b34a-8d8ab87e1d7f)


---

## Security notes
- Use App Passwords instead of your main password. Revoke the App Password when no longer needed.  

---

## Limitations & risks
- Script was tested **only on Gmail**. Other providers may require small modifications.  
- Rapid, high-volume sending increases the risk of rate-limits and spam classification. Stay below provider limits (Gmail ≈ 500/day).  
- If you need to send large volumes reliably, use a transactional email service (SendGrid, Mailgun, Amazon SES).

---

## Suggested improvements (Maybe in the Future)
- OAuth2 authentication instead of App Passwords.  
- Retry logic with exponential backoff on failures.  
- Per-recipient templating (mail merge).  
### But ther's no Time for That

---

## License & responsibility
Use this tool responsibly. I'm not responsible for misuse. Ensure compliance with local laws and service terms.

---

## Quick start
1. Prepare `signin.txt`, `Mails.txt`, `Message.txt`, and `Attachment/`.  
2. Install dependencies.  
3. Run the script:
```bash
python G-Mail.py
```
4. Check the `Result/` folder for `Result.txt` and `Result.xlsx`.


## For Those People that didn't know How to run this Code:

If you’ve never used Python before — don’t worry. Just follow these simple steps carefully:

1. **Install Python (Version 3.8 or newer)**
   - Go to [https://www.python.org/downloads/](https://www.python.org/downloads/)
   - Click **Download Python** and install it.
   - During installation, make sure to **check the box that says “Add Python to PATH”** before clicking *Install Now*.

2. **Open the program folder**
   - Locate the folder where this project (and the file `G-Mail.py`) is saved.
   - Hold **Shift** on your keyboard, then **right-click** in an empty space inside the folder.
   - From the context menu, choose **“Open Command Window Here”** or **“Open in Terminal”**.  
     → This will open the command prompt directly inside your project folder.

3. **Create and activate a virtual environment (called `myenv`)**
   ```bash
   python -m venv myenv
   ```
   - On **Windows:**
     ```bash
     myenv\Scripts\activate
     ```
   - On **macOS:**
     ```bash
     source myenv/bin/activate
     ```
	- On **Linux:**
	  `of course, you already know what to do!`

4. **Install all requirements**
   Make sure the file `requirements.txt` is in the same folder, then type:
   ```bash
   pip install -r requirements.txt
   ```
   - On **Windows:** Maybe you need to run:
   ```
   python.exe -m pip install --upgrade pip 
   ```

5. **Prepare your files**
   Make sure the following files/folders exist in the same directory as the script:
   - `signin.txt`
   - `Mails.txt`
   - `Message.txt`
   - A folder named `Attachment` containing your PDF or other files to send.

6. **Run the program**
   ```bash
   G-Mail.py
   ```
   or
   ```bash
   python G-Mail.py
   ```
   or
   ```bash
   python3 G-Mail.py
   ```

8. **Check your results**
   - After sending, a new folder called `Result` will be created automatically.
   - It contains:
     - `Result.txt` → text summary
     - `Result.xlsx` → Excel report showing sent/failed emails and timestamps.

That’s it! You’re done. Your bulk mailer will now send emails automatically. 🚀

## If you encounter any problems, feel free to reach out to me:  
- **Email:** eslaam.mustafa@gmail.com
- **LinkedIn:** [https://www.linkedin.com/in/eslamabomandour/](https://www.linkedin.com/in/eslamabomandour/)  




