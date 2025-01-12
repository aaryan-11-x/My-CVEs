### **Description:**
A vulnerability exists in the `/admin/change-image.php` endpoint of Travel Management System 1.0, allowing an authenticated user to upload arbitrary PHP files without proper validation. This enables remote code execution (RCE) on the server.

### **Steps to Reproduce:**
1. Login to the application as an admin. Default credentials are:
   - Username: `admin`
   - Password: `Test@123`
2. Navigate to `/admin/manage-packages.php`.
![File Upload](https://github.com/user-attachments/assets/f26adcbe-6194-49c5-807c-2972ca045b0e)

3. Click on "View Details" for any package to be redirected to `/admin/change-image.php?pid=[number]`.
![File Upload - 0](https://github.com/user-attachments/assets/1e3804eb-1021-4070-b7ce-b95a62fffb02)

4. Use the "New Image" option to upload a malicious PHP shell file (e.g., `shell.php` containing `<?php echo system($_GET['cmd']); ?>`).
![File Upload - 1](https://github.com/user-attachments/assets/d99ab31b-559d-4a42-abb4-2673f0ae3f67)

5. Access the uploaded shell at `/admin/packageimages/[php-shell-name]` and execute arbitrary commands via the `cmd` parameter.
![File Upload - 2](https://github.com/user-attachments/assets/c13731dd-9a19-485d-b8a5-abd2f2540aff)


### **Impact:**
This vulnerability allows attackers to:
- Execute arbitrary commands on the server.
- Compromise sensitive data.
- Escalate privileges and potentially take full control of the server.

### **Suggested Fix:**
1. Restrict allowed file types to only legitimate image formats (e.g., `.jpg`, `.png`, `.jpeg`).
2. Validate file contents to ensure uploaded files match their declared MIME types.
3. Store uploaded files in non-executable directories.
4. Sanitize file names to prevent directory traversal and code execution.

---

### **References:**
- [Travel Management System 1.0 Project Page](https://code-projects.org/travels-management-system-using-php-source-code/)
### **Credits:**
These vulnerabilities were discovered by **Aaryan Golatkar**.

