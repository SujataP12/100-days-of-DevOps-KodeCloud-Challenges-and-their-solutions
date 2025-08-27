## Day 76 – Jenkins Project Security 🔒🛡️

### 🔎 Why Project Security?
By default, Jenkins jobs can be edited or executed by anyone with access to Jenkins.  
**Project Security** allows you to:
- Control who can **configure, build, or delete jobs**  
- Assign permissions to **users or groups**  
- Protect sensitive pipelines and credentials  

---

## 🐧 Step 1: Enable Security in Jenkins
1. Go to **Manage Jenkins → Configure Global Security**  
2. Enable **Jenkins’ own user database** or use **LDAP** for centralized authentication  
3. Enable **Authorization**:
   - **Matrix-based security** (built-in)  
   - **Role-Based Strategy** (plugin)  

---

## 🐧 Step 2: Matrix-based Security for Projects
1. Go to **Manage Jenkins → Configure Global Security → Authorization → Matrix-based security**  
2. Add users or groups  
3. Assign **permissions per project**:
| Permission Type | Description |
|-----------------|-------------|
| Overall/Read | Access Jenkins dashboard |
| Job/Build | Run builds |
| Job/Configure | Modify job configuration |
| Job/Delete | Delete jobs |
| Job/Workspace | Access workspace files |
| SCM/Tag | Access source control operations |

4. Example:
- Admin → Full permissions  
- Developer → Build & read only  
- QA → Read & test execution  

---

## 🐧 Step 3: Role-Based Strategy (Optional)
1. Install **Role-Based Authorization Strategy plugin**  
2. Go to **Manage Jenkins → Manage and Assign Roles → Roles**  
3. Create roles:
   - **Admin** → Full access  
   - **Dev** → Build, configure, and read jobs  
   - **QA** → Read-only or limited build access  
4. Assign users or groups to roles  

---

## 🐧 Step 4: Configure Job-Specific Security
- Open a job → **Configure → Enable project-based security**  
- Assign **permissions per user or group**:
  - Build  
  - Configure  
  - Read  
  - Workspace access  

---

## 🐧 Step 5: Verify Security
- Logout → Login as different users  
- Confirm access is restricted according to the assigned roles  

---

## ✅ Summary
- Jenkins Project Security protects jobs from **unauthorized access**  
- Matrix-based security or Role-Based Strategy can be used  
- Assign **permissions carefully** for master & slave nodes  
- Essential for **secure CI/CD pipelines** 🚀
