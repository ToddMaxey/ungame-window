# ungame-window
How to disabled, or remove the built in games on Windows 10 or 11

**Detailed Methods to Remove or Prevent the Use of Built-in Games on Supported Windows Machines**

---

Built-in games on Windows machines, such as **Microsoft Solitaire Collection**, **Candy Crush Saga**, and others, can be distracting or unwanted in certain environments like schools, workplaces, or for parental control purposes. Below are comprehensive methods to remove or prevent the use of these built-in games on currently supported Windows versions (Windows 10, Windows 11, and Windows Server editions). Each method includes detailed steps and specific information about the applications involved.

---

### **1. Uninstalling Games via Settings**

**Applicable to:** Windows 10, Windows 11

**Common Built-in Games:**

- **Microsoft Solitaire Collection**
- **Microsoft Mahjong**
- **Microsoft Minesweeper**
- **Candy Crush Saga**
- **Bubble Witch 3 Saga**
- **March of Empires**
- **Disney Magic Kingdoms**
- **Hidden City**

**Steps:**

1. **Open Settings:**

   - Press **Windows key + I** to open the **Settings** app.

2. **Navigate to Apps:**

   - Click on **Apps** > **Apps & features**.

3. **Find the Game:**

   - Use the search bar under **Apps & features** to search for the specific game (e.g., type **"Solitaire"**).

4. **Uninstall:**

   - Click on the game entry in the list.
   - Click the **Uninstall** button.
   - Confirm the action when prompted by clicking **Uninstall** again.

**Notes:**

- Some built-in games may not be uninstallable through this method, especially if they are system apps.
- This method removes the game for the current user only.

---

### **2. Using PowerShell to Uninstall Built-in Games**

**Applicable to:** Windows 10, Windows 11

**Common Built-in Game Package Names:**

- **Microsoft Solitaire Collection:** `Microsoft.MicrosoftSolitaireCollection`
- **Microsoft Mahjong:** `Microsoft.MicrosoftMahjong`
- **Microsoft Minesweeper:** `Microsoft.MicrosoftMinesweeper`
- **Candy Crush Saga:** Package names may vary (e.g., `king.com.CandyCrushSaga`)
- **Other games:** Similar naming conventions.

**Steps:**

1. **Open PowerShell as Administrator:**

   - Right-click on the **Start** button and select **Windows PowerShell (Admin)** or **Windows Terminal (Admin)**.

2. **List Installed Packages:**

   - Run the following command to list all installed AppX packages for the current user:

     ```powershell
     Get-AppxPackage | Select Name, PackageFullName
     ```

   - To list packages for all users:

     ```powershell
     Get-AppxPackage -AllUsers | Select Name, PackageFullName
     ```

3. **Identify the Game Package:**

   - Look for the package names associated with the games you wish to remove.

4. **Uninstall the Game:**

   - Run the following command, replacing `<PackageName>` with the actual package name:

     ```powershell
     Get-AppxPackage -AllUsers <PackageName> | Remove-AppxPackage -AllUsers
     ```

   - **Examples:**

     - **Remove Microsoft Solitaire Collection:**

       ```powershell
       Get-AppxPackage -AllUsers Microsoft.MicrosoftSolitaireCollection | Remove-AppxPackage -AllUsers
       ```

     - **Remove Microsoft Minesweeper:**

       ```powershell
       Get-AppxPackage -AllUsers Microsoft.MicrosoftMinesweeper | Remove-AppxPackage -AllUsers
       ```

     - **Remove Candy Crush Saga (using wildcard due to varying package names):**

       ```powershell
       Get-AppxPackage -AllUsers *CandyCrush* | Remove-AppxPackage -AllUsers
       ```

**Notes:**

- Using `-AllUsers` removes the app for all user accounts on the machine.
- Be cautious when using wildcards to avoid removing unintended packages.
- Some system apps may be protected and cannot be removed.

---

### **3. Disabling Games via Windows Features**

**Applicable to:** Windows 10, Windows 11, Windows Server

**Steps:**

1. **Open Windows Features Dialog:**

   - Press **Windows key + R**, type `optionalfeatures`, and press **Enter**.

2. **Modify Windows Features:**

   - In the **Windows Features** dialog, look for entries related to **Games**.

     - **Note:** In Windows 10 and 11, built-in games are typically not listed here as they are delivered as apps.

3. **Disable Games (if listed):**

   - Uncheck the boxes next to the games you want to disable.

4. **Apply Changes:**

   - Click **OK** and allow Windows to make the necessary changes.
   - Restart the computer if prompted.

**Notes:**

- This method is more applicable to older versions of Windows or Windows Server editions where games are included as features.

---

### **4. Using Group Policy Editor**

**Applicable to:** Windows Pro, Enterprise, Education editions; Windows Server

**Common Game Executable Names:**

- **Solitaire:** `solitaire.exe`, `solitairecollection.exe`
- **Mahjong:** `mahjong.exe`
- **Minesweeper:** `minesweeper.exe`
- **Hearts:** `mshearts.exe`
- **Spider Solitaire:** `spider.exe`

**Steps:**

1. **Open Group Policy Editor:**

   - Press **Windows key + R**, type `gpedit.msc`, and press **Enter**.

2. **Navigate to the Policy Path:**

   - **User Configuration** > **Administrative Templates** > **System**

3. **Enable "Don't Run Specified Windows Applications":**

   - In the right pane, find and double-click **"Don't run specified Windows applications"**.

4. **Configure the Policy:**

   - Set the policy to **Enabled**.

5. **Specify Disallowed Applications:**

   - Click on the **Show** button under **Options**.

   - In the **Show Contents** dialog, add the executable names of the games you wish to block.

     - **Example entries:**

       - `solitaire.exe`
       - `solitairecollection.exe`
       - `mahjong.exe`
       - `minesweeper.exe`
       - `mshearts.exe`

6. **Apply Settings:**

   - Click **OK** to close dialogs and apply settings.

7. **Update Group Policy:**

   - Open **Command Prompt** as administrator and run:

     ```cmd
     gpupdate /force
     ```

**Notes:**

- This policy prevents specified applications from running.
- The executable names must match the actual file names of the games.
- **Group Policy Editor** is not available in Windows Home editions.

---

### **5. Using AppLocker**

**Applicable to:** Windows Enterprise and Education editions; Windows Server

**Steps:**

1. **Open Local Security Policy Editor:**

   - Press **Windows key + R**, type `secpol.msc`, and press **Enter**.

2. **Navigate to AppLocker:**

   - **Security Settings** > **Application Control Policies** > **AppLocker**

3. **Create New Executable Rules:**

   - Right-click on **Executable Rules** and select **Create New Rule**.

4. **Set Up the Rule:**

   - **Before You Begin:** Click **Next**.

   - **Permissions:** Choose **Deny**.

   - **User or Group:** Click **Select** and choose **Everyone** or specific user groups.

5. **Specify Conditions:**

   - **Condition Type:** Choose **Publisher**, **Path**, or **File Hash**.

   - **Using Path Condition:**

     - **Path:** Enter the path to the game's executable.

       - For built-in games installed via Microsoft Store, use:

         ```plaintext
         %ProgramFiles%\WindowsApps\*<PackageName>*
         ```

       - **Example:**

         ```plaintext
         %ProgramFiles%\WindowsApps\Microsoft.MicrosoftSolitaireCollection_*\*
         ```

6. **Create the Rule:**

   - Click **Next** and then **Create**.

7. **Enable the Application Identity Service:**

   - Open **Services** (type `services.msc` in Run dialog).

   - Find **Application Identity** service.

   - Set its **Startup type** to **Automatic**.

   - Start the service.

**Notes:**

- **AppLocker** provides granular control over which applications can run.
- Not available in Windows Home or Pro editions.
- For Microsoft Store apps, AppLocker rules can be complex due to the way apps are packaged.

---

### **6. Editing the Registry**

**Applicable to:** All Windows editions

**Steps:**

1. **Open Registry Editor:**

   - Press **Windows key + R**, type `regedit`, and press **Enter**.

2. **Navigate to the Policy Key:**

   - For current user:

     ```plaintext
     HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer
     ```

   - For all users:

     ```plaintext
     HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer
     ```

3. **Create or Modify "DisallowRun" Key:**

   - Right-click on **Explorer**, select **New** > **Key**, and name it **DisallowRun**.

4. **Enable "DisallowRun" Policy:**

   - In the **Explorer** key, create a new **DWORD (32-bit) Value** named **DisallowRun** and set its value to **1**.

5. **Add String Values for Each Game:**

   - Within the **DisallowRun** key, create new **String Values** named `1`, `2`, `3`, etc.

   - Set their data to the executable names of the games to block.

     - **Example:**

       - **Name:** `1` **Data:** `solitairecollection.exe`
       - **Name:** `2` **Data:** `mahjong.exe`

6. **Apply Changes:**

   - Restart the computer or log off and log back in.

**Notes:**

- This method prevents specified applications from running.
- Be cautious when editing the registry; incorrect changes can cause system issues.
- Back up the registry before making changes.

---

### **7. Using Parental Controls (Family Safety)**

**Applicable to:** Windows 10, Windows 11

**Steps:**

1. **Set Up a Child Account:**

   - Go to **Settings** > **Accounts** > **Family & other users**.

   - Click **Add a family member** and choose **Add a child**.

2. **Manage Family Settings Online:**

   - Visit [https://account.microsoft.com/family](https://account.microsoft.com/family) and sign in.

3. **Restrict Apps and Games:**

   - Select the child's account.

   - Click on **Content filters**.

   - Under **Apps and games**, set the age limit to restrict games above a certain rating.

   - To block specific games:

     - Scroll to **Blocked apps**.

     - Add the games you wish to block.

**Notes:**

- Suitable for parental control to restrict access for child accounts.
- Requires Microsoft accounts for both parent and child.
- Restrictions apply only to child accounts, not administrator accounts.

---

### **8. Using Third-Party Software**

**Applicable to:** All Windows editions

**Options:**

- **Application Control Software:**

  - **Faronics Anti-Executable**

  - **Symantec Endpoint Protection**

  - **AppGuard**

- **Uninstallation Tools:**

  - **CCleaner**

  - **Revo Uninstaller**

  - **IObit Uninstaller**

**Steps:**

1. **Install the Software:**

   - Download and install from a reputable source.

2. **Configure Application Blocking or Removal:**

   - Use the software to block or uninstall the games.

**Notes:**

- Ensure compatibility with your Windows version.
- Be aware of licensing requirements.
- Third-party tools may offer more advanced features but require additional management.

---

### **9. Deploying Changes via System Imaging or Deployment Tools**

**Applicable to:** Enterprise environments

**Tools:**

- **Deployment Image Servicing and Management (DISM)**
- **Microsoft Deployment Toolkit (MDT)**
- **System Center Configuration Manager (SCCM)**
- **Windows Imaging and Configuration Designer (Windows ICD)**

**Steps:**

1. **Customize a Windows Image:**

   - Use **DISM** to mount the Windows image file.

     ```cmd
     dism /Mount-WIM /WimFile:C:\Images\install.wim /Index:1 /MountDir:C:\Mount
     ```

2. **List Installed Packages:**

   - Run:

     ```cmd
     dism /Image:C:\Mount /Get-ProvisionedAppxPackages
     ```

3. **Remove Packages:**

   - Identify the package names of the games.

   - Use:

     ```cmd
     dism /Image:C:\Mount /Remove-ProvisionedAppxPackage /PackageName:<PackageFullName>
     ```

   - **Example:**

     ```cmd
     dism /Image:C:\Mount /Remove-ProvisionedAppxPackage /PackageName:Microsoft.MicrosoftSolitaireCollection_4.9.7040.0_neutral_~_8wekyb3d8bbwe
     ```

4. **Commit Changes:**

   - Unmount the image and commit changes:

     ```cmd
     dism /Unmount-WIM /MountDir:C:\Mount /Commit
     ```

5. **Deploy the Customized Image:**

   - Use **MDT** or **SCCM** to deploy the image to client machines.

**Notes:**

- Suitable for large-scale deployments.
- Requires advanced knowledge of Windows deployment.
- Ideal for applying changes across multiple machines consistently.

---

### **Considerations and Best Practices**

- **Backup Data:**

  - Always back up important data before making system changes.

- **Test Changes:**

  - Test on a single machine before wide deployment.

- **User Communication:**

  - Inform users about the changes to avoid confusion.

- **Policy Compliance:**

  - Ensure compliance with organizational policies and licensing agreements.

- **Regular Updates:**

  - Keep systems updated to maintain security and functionality.

- **Monitoring:**

  - Monitor for reinstallation of games, especially via Windows Update or Microsoft Store.

---

### **Additional Information**

**Common Built-in Games and Their AppX Package Names:**

- **Microsoft Solitaire Collection:**

  - Package Name: `Microsoft.MicrosoftSolitaireCollection`

- **Microsoft Mahjong:**

  - Package Name: `Microsoft.MicrosoftMahjong`

- **Microsoft Minesweeper:**

  - Package Name: `Microsoft.MicrosoftMinesweeper`

- **Candy Crush Saga:**

  - Package Names may vary:

    - `king.com.CandyCrushSaga`
    - `king.com.CandyCrushSodaSaga`

- **Bubble Witch 3 Saga:**

  - Package Name: `king.com.BubbleWitch3Saga`

**Finding Package Names:**

- Use PowerShell to list all packages:

  ```powershell
  Get-AppxPackage -AllUsers | Select Name, PackageFullName
  ```

- Review the output to identify the exact package names.

---

### **Conclusion**

By following these detailed methods, you can effectively remove or prevent the use of built-in games on Windows machines. Choose the method that best suits your environment and requirements, whether it's a single home PC or a large enterprise network.

---

**If you have any further questions or need assistance with specific steps or scenarios, feel free to ask!**
