# 🔒 Complete Security Implementation Prompt for Investor Dashboard with PII Encryption

Use this prompt at the start of your project to implement enterprise-grade security with proper PII encryption, role-based access control, and compliance features.

---

## **Implementation Request**

"I need you to implement a comprehensive security architecture for an investor dashboard application with the following requirements:

### **1. Role-Based Access Control (RBAC)**

Create a secure role system with three roles:
- `investor` (default role for all new users)
- `admin` (full system access)
- `compliance_officer` (audit and compliance access)

**Critical Security Requirements:**
- Roles MUST be stored in a separate `user_roles` table, NOT on the profiles table
- Use a `SECURITY DEFINER` function called `has_role()` to check roles (prevents RLS infinite recursion)
- Never check admin status using client-side storage (localStorage/sessionStorage) or hardcoded credentials
- Always use server-side validation with proper authentication

**Why This Matters:**
- Storing roles on profiles allows users to modify their own roles (privilege escalation attack)
- Direct RLS queries to user_roles cause infinite recursion
- Client-side role checks can be manipulated by attackers using browser dev tools
- A compromised encryption key would require rotating the key and re-encrypting all data

### **2. PII Encryption System**
... (content truncated for brevity) ...
