## Day 22 – Configure Jenkins User Access 👤🔐

### 🔎 Why Configure User Access?
By default, Jenkins runs with **open access** after installation.  
To secure your CI/CD pipelines, you must configure **user authentication** and **authorization** so only authorized users can manage jobs, pipelines, and credentials.  

---

## 🐧 Step 1: Access Jenkins
Open in browser:
```bash
http://<your_server_ip>:8080
```
Login with your **initial admin password**:
```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
## Step 2: Enable Security

**Go to Manage Jenkins → Security → Configure Global Security**

Under Access Control:
Enable Jenkins’ own user database
Select Allow users to sign up (optional)
Enable Matrix-based security or Role-based Strategy (requires plugin)

## Step 3: Create New Jenkins Users

Go to Manage Jenkins → Manage Users → Create User

Enter:

Username 👤

Password 🔑

Full Name 🏷️

Email 📧

## Step 4: Assign Roles & Permissions

#### Option 1 – Matrix-based Security

Grant Read to anonymous

Assign Admin permissions to your admin user

Assign limited permissions to developers/operators

#### Option 2 – Role-based Authorization (requires plugin role-strategy)

Install Role-based Authorization Strategy plugin

Go to Manage Jenkins → Manage and Assign Roles

Create roles (Admin, Dev, Read-only)

Assign users/groups to roles

## Step 5: Setup LDAP in Jenkins

### 🔎 What is LDAP?
**LDAP (Lightweight Directory Access Protocol)** is used to access and manage directory services such as **Active Directory (AD)** or **OpenLDAP**.  
By integrating LDAP with Jenkins, you can:
- Use **corporate credentials** for login  
- Apply **centralized authentication**  
- Manage **role-based access** easily  

---

## 🐧 Step I: Install LDAP Plugin in Jenkins
1. Go to **Manage Jenkins → Plugins → Available Plugins**  
2. Search for **LDAP Plugin**  
3. Install and restart Jenkins  

---

## 🐧 Step II: Configure LDAP in Jenkins
1. Go to **Manage Jenkins → Configure Global Security**  
2. Under **Security Realm**, select **LDAP**  
3. Provide your LDAP/AD server details:  

| Field | Example Value |
|-------|---------------|
| **Server** | `ldap://ldap.example.com:389` |
| **Root DN** | `dc=example,dc=com` |
| **User search base** | `ou=users,dc=example,dc=com` |
| **User search filter** | `uid={0}` *(for OpenLDAP)* OR `sAMAccountName={0}` *(for AD)* |
| **Manager DN** | `cn=admin,dc=example,dc=com` |
| **Manager Password** | `SuperSecretPassword` |

4. Test LDAP connection ✅  

---

## 🐧 Step III: Configure Authorization
- Enable **Matrix-based security** or **Role-based strategy**  
- Assign roles/permissions to LDAP users or groups  

Example:
- `devops-team` → build & deploy access  
- `admins` → full access  
- `read-only` → view jobs/logs  

---

## 🐧 Step IV: Verify Login
- Logout of Jenkins  
- Login using your **LDAP/AD credentials**  
- Confirm roles & permissions are applied correctly  

---

## ⚡ Optional: Secure with LDAPS (SSL/TLS)
For secure authentication:
- Use `ldaps://ldap.example.com:636`  
- Import LDAP server certificate into Java keystore:  
```bash
sudo keytool -import -trustcacerts -alias ldapserver \
  -file /path/to/cert.pem -keystore $JAVA_HOME/jre/lib/security/cacerts
```
