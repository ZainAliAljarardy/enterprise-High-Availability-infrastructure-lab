# Troubleshooting Documentation

## Issue 1: Failed to Add Server in Windows Admin Center (WAC) Due to Account Permissions

### Description
When attempting to add a new server (such as `dc.zfood.local`) inside the **Windows Admin Center** interface, a connection confirmation error occurs, accompanied by an "Access Denied" message in the background.

### Error Messages
* **In the Add Server panel:** > `You can add this server to your list of connections, but we can't confirm it's available.`
* **In the background page:** > `Access denied: Sorry, but you are not authorized to make this request.`

### Cause
This issue occurs because the **Windows Admin Center (WAC)** was installed or is being run under a **Local Account** instead of a **Domain Admin Account**. As a result, the application lacks the necessary Kerberos or NTLM elevation privileges to query, manage, and authenticate against target infrastructure servers within the Active Directory domain environment.

### Screenshot
![Windows Admin Center Access Denied Error](error-becouse-the%20WAC-have-been-download-by-local-account-not-domain-admin-account.png)

---

### Resolution
To permanently resolve this access restriction, the application deployment context must be changed from a local workspace to the domain administration tier by following these steps:

1. **Uninstall Current Instance:** Go to **Control Panel > Programs and Features**, select **Windows Admin Center**, and perform a full uninstallation of the software.
2. **Elevate Installation Context:** Log out of the local server machine account and log back in using a dedicated account with **Domain Admins** privileges (e.g., `ZFOOD\Administrator`).
3. **Reinstall WAC:** Launch the Windows Admin Center installer package again while logged in as the Domain Admin to ensure the core Gateway Service registers under the correct active directory administrative scope.
4. **Verification:** Open the newly installed WAC console, navigate to the Add Connections panel, and re-add `dc.zfood.local`. The target will now pass structural confirmation seamlessly.
