# Network File Shares and Permissions Walkthrough

This walkthrough demonstrates how to configure shared folders, assign permissions, test access from a domain-joined client, and use Active Directory security groups to control access to network resources.

---

## Table of Contents
- [Accessing Virtual Machines](#accessing-virtual-machines)
- [Creating File Share Folders](#creating-file-share-folders)
- [Configuring Share Permissions](#configuring-share-permissions)
- [Testing Access as a Standard User](#testing-access-as-a-standard-user)
- [Configuring Accounting Group Permissions](#configuring-accounting-group-permissions)
- [Testing Accounting Share Access](#testing-accounting-share-access)
- [Optional Screenshots Section](#optional-screenshots-section)

---

## Accessing Virtual Machines

Begin by logging in to both the domain controller and the domain-joined workstation.

**Log in to DC-1 as:**  
`mydomain.com\jane_admin` (Domain Admin)

**Log in to Client-1 as:**  
A standard domain user account  
`mydomain\<someuser>`

These two machines will be used throughout the lab to configure and test file permissions.

---

## Creating File Share Folders

On DC-1, prepare the folders that will demonstrate different permission levels.

Create the following directories on the **C:** drive:

- **read-access**  
- **write-access**  
- **no-access**  
- **accounting**  
  (permissions configured later)

These folders will be shared and configured with different permission sets.

---

## Configuring Share Permissions

Apply basic share permissions to each folder.

### **read-access**
- Group: **Domain Users**  
- Permission: **Read**

### **write-access**
- Group: **Domain Users**  
- Permissions: **Read/Write**

### **no-access**
- Group: **Domain Admins**  
- Permissions: **Read/Write**

The **accounting** folder is intentionally left unchanged until a later step.

---

## Testing Access as a Standard User

Use Client-1 to verify which shares are accessible based on the permissions assigned.

Steps:
1. On Client-1, open Run (Windows + R).  
2. Enter:  
   `\\dc-1`
3. Attempt to open each shared folder.
4. Observe:
   - Which folders can be opened  
   - Which allow file creation  
   - Which correctly deny access  

This confirms that permissions applied on DC-1 work as intended for normal users.

---

## Configuring Accounting Group Permissions

Now create a security group to manage access to the restricted share.

Steps on DC-1:
1. Open **Active Directory Users and Computers (ADUC)**.  
2. Create a new security group: **ACCOUNTANTS**  
3. Apply folder permissions to the **accounting** directory:  
   - Group: **ACCOUNTANTS**  
   - Permissions: **Read/Write**  

This ensures that only users in this group will be able to access the accounting share.

---

## Testing Accounting Share Access

Verify that access to the accounting share behaves correctly based on group membership.

Steps:
1. On Client-1 (logged in as `<someuser>`), attempt to access the **accounting** share.  
   - Access should **fail**  
2. Return to DC-1 and add `<someuser>` to the **ACCOUNTANTS** group.  
3. On Client-1, sign out and sign back in.  
4. Try accessing the **accounting** share again.  
   - Access should now **succeed**

This confirms that AD security groups control access effectively.
