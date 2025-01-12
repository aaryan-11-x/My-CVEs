## **Description**  
A stored cross-site scripting (XSS) vulnerability exists in the `/admin/manage-pages.php` endpoint of the **Online Car Rental System**. The `pgedetails` parameter allows unfiltered HTML input, which is rendered directly on the `/page.php?type=faqs` page. This vulnerability can be exploited by an authenticated attacker to execute arbitrary JavaScript in the context of other users' browsers, leading to data theft, redirection, or account compromise.

---

## **Details**  

### **Endpoint:**  
`/admin/manage-pages.php`

### **Parameter:**  
`pgedetails`

### **Vulnerable Code Snippet:**  
```php
<p><?php echo $result->detail; ?></p>
```

The `echo` statement outputs user-supplied input (`$result->detail`) without any sanitization, leading to the execution of JavaScript payloads.

### **Impact:**  
1. **Stored XSS**: Malicious scripts are stored on the server and executed whenever a user visits the page displaying the `pgedetails` parameter.
2. **Potential for Data Theft**: Attackers can steal cookies, session tokens, or other sensitive data.
3. **User Redirection**: Malicious scripts can redirect users to phishing or malicious websites.
4. **Site Defacement**: Attackers can alter page contents, impacting trust in the application.

---

## **Steps to Reproduce**  

### **1. Login as Admin:**  
Access the `/admin/manage-pages.php` endpoint using valid credentials.

### **2. Modify Page Details:**  
Select the "FAQs" section and update the `pgedetails` parameter with a malicious payload, such as:  
```html
<script>alert(1)</script>
```

### **3. Observe the Impact:**  
Visit `/page.php?type=faqs` to view the stored payload. The injected JavaScript code will execute, demonstrating the vulnerability.

---

## **Remediation**  
Sanitize the `pgedetails` input using PHP's `htmlentities()` function to encode special characters:  
```php
<p><?php echo htmlentities($result->detail, ENT_QUOTES); ?></p>
```

---

## **Acknowledgments**  
Reported by Aaryan Golatkar.

---

## **References**  
- [Project Link](https://code-projects.org/online-car-rental-using-php-source-code/)  
