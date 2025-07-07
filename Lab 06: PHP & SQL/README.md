# Lab 06: PHP & SQL

## Lab Objectives
1. Develop and deploy PHP scripts with dynamic content.
2. Exploit file inclusion vulnerabilities.
3. Manage MySQL databases and user permissions.

## Key Activities
- Created `oabolurin_test.php` with `echo`, `print`, and `printf()`.
- Uploaded files via FTP to Ubuntu server.
- Demonstrated insecure file inclusion (`accounts.txt`).
- Configured MySQL users with granular privileges.


# Lab 06 Findings

## Vulnerabilities Identified
1. **File Inclusion**:  
   - Unrestricted `include()` exposed `accounts.txt`.  
   - *Mitigation*: Use `allow_url_include=Off` in `php.ini`.

2. **MySQL Privilege Escalation**:  
   - Overprivileged users (`ALL PRIVILEGES`).  
   - *Mitigation*: Apply principle of least privilege.

## Key Takeaways
- PHP scripts must validate file paths.
- Database users should have minimal required access.
- FTP transfers should use encryption (SFTP/FTPS).
