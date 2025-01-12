Endpoint: /admin/manage-pages.php
Parameter: pgedetails

Description:
The pgedetails field allows unfiltered HTML input, leading to Stored XSS when viewing the updated page.

Steps to Reproduce:
Login as an admin.
Navigate to /admin/manage-pages.php and select "FAQs."
Update the page details with:
<script>alert(1)</script>
View the page at /page.php?type=faqs to observe the script execution.
Impact:
An attacker can inject malicious scripts to steal cookies, redirect users, or deface the site.

Suggested Fix:
Sanitize the output with:

echo htmlentities($result->detail, ENT_QUOTES);