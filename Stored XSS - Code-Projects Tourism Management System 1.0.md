### **Description:**
A stored cross-site scripting (XSS) vulnerability exists in the `/page.php?type` endpoint due to the insecure handling of user input in the `pgedetails` parameter. An attacker can inject malicious JavaScript code, which is executed whenever the affected page is viewed.

### **Steps to Reproduce:**
1. Login to the application as an admin.
2. Navigate to `/admin/manage-pages.php` and select any page for editing.
![XSS - 1](https://github.com/user-attachments/assets/554d26ac-ae29-470e-bdf9-66eab9308689)

3. In the "Package Details" field, enter arbitrary details and capture the request in BurpSuite.
4. Modify the `pgedetails` parameter to include a malicious payload such as:
   ```html
   <script>alert(1)</script>
   ```
![XSS - 2](https://github.com/user-attachments/assets/7fb440d7-f527-4a1c-af36-e13f8c88fdd1)

5. Submit the modified request.
6. Navigate to `/page.php?type=[malicious-page]` to observe the execution of the injected JavaScript payload.
![XSS - 3](https://github.com/user-attachments/assets/11e762cd-d2bc-49fb-b9ba-44d53b12202d)


### **Impact:**
This vulnerability enables attackers to:
- Steal sensitive information (e.g., cookies, session tokens).
- Redirect users to malicious websites.
- Execute actions on behalf of other users, potentially leading to account compromise.

### **Suggested Fix:**
1. Sanitize output using functions like `htmlentities()` or `htmlspecialchars()` with `ENT_QUOTES` to encode HTML special characters.
2. Validate and sanitize user input on both client-side and server-side to reject malicious scripts.
3. Implement a Content Security Policy (CSP) to mitigate the impact of potential XSS attacks.

---

### **References:**
- [Travel Management System 1.0 Project Page](https://code-projects.org/travels-management-system-using-php-source-code/)

---

### **Credits**
These vulnerabilities were discovered by **Aaryan Golatkar**.
