# Free syncing feature for Obsidian !

*This is only tested in Windows and IOS, 
Feel free to take the idea and implemented it yourself !*

### Okay, what is the concept of this ?
- *Firstly this is scenario where your main obsidian is in windows desktop, and you wanted to access it from other device, like iphone*
- My solution to sync between different device is using two concept and tools :
	- **GitHub Repo** for **Windows**, and **other PC**
		- This idea is pretty simple, using one of community plugin, [obdisian-git](https://github.com/denolehov/obsidian-git), we could automatically pull repo and push our obsidian
	- **Cloud Storage** (**ICloud, or other**) with **batch file** and **automatic task scheduler**
		- In this solution, I only use **ICloud** to sync my **Window Desktop** and **Iphone 11**
		- The concept is, by connecting **ICloud** and **Windows** using **icloud app** and getting direct access to the **cloud storage**, we could make **script** that **automatically**, **first delete all file & folder** (except .obsidian file), then **copy your desktop obsidian folder** to **ICloud / Cloud Storage**

## Initial Configuration
1. Download **obsidian-git** community plugin, and then setup the automatic **pull & push** repo by following their **simple** guide
2. Change all you default created file directory to some "main folder", in this case, i make a main folder named [[#Default Configuration Folder directory|"--HOME--" ]]
3. After that, initialize your obsidian on your phone, here I used **iphone 11**, in your **cloud storage**, like **icloud**
4. Download and connect your **cloud storage** into your **desktop** 
	- I use **icloud app** so I could access my **cloud directory**
5. Make a **batch file** using this command, and please change your directory with the correct file folder :
	##### Format  file-sync.bat
	*vault-dir-destination : Cloud storage directory
	vault-dir-source : Main obsidian directory
	file name : yourBatchFileName.bat
	file.txt : your excluded txt file*
	
	```
	FOR /D %%i IN ("vault-dir-destination\*") DO RD /S /Q "%%i" DEL /Q "vault-dir-destination\*.*"
	xcopy "vault-dir-source\*" "vault-dir-destination\"  /Y /E /D /C /F /I /Z /J /S /K /R /Q /EXCLUDE:file.txt
	```
	##### Example obsidian-sync.bat
	*vault-dir-destination : `C:\xxx\iCloudDrive\iCloud~md~obsidian\vault-destination`
	vault-dir-source : `E:\Obsidian\vault-source`
	File name : `obsidian-sync.bat`
	file.txt : `fileObsidianIgnore.txt`*
	
	```
	FOR /D %%i IN ("C:\xxx\iCloudDrive\iCloud~md~obsidian\vault-destination\*") DO RD /S /Q "%%i" DEL /Q "C:\xxx\iCloudDrive\iCloud~md~obsidian\vault-destination*.*"
	xcopy "E:\Obsidian\vault-source\*" "C:\xxx\iCloudDrive\iCloud~md~obsidian\vault-destination\"  /Y /E /D /C /F /I /Z /J /S /K /R /Q /EXCLUDE:fileObsidianIgnore.txt
	```

6. Make an **exclude file list**
	##### Format  file.txt
	
	```
	\.obsidian
	\.git
	\[folder-name]
	.format-file\
	etc
	```
	##### Example fileObsidianIgnore.txt`*
	
	```
	\.obsidian
	\.git
	```

7. Make an **.vbs script**, so when the `obsidian-sync.bat` get executed, there is no terminal open up on your screen
	##### Format  silent-sync.vbs
	*obsidian-sync-bat-location: Your bat file location*
	
	```
	Set WshShell = CreateObject("WScript.Shell") 
	WshShell.Run chr(34) & "obsidian-sync-bat-location" & Chr(34), 0
	Set WshShell = Nothing
	```
	##### Example silent-sync.vbs
	*obsidian-sync-bat-location : `E:\Automation\obsidian-sync.bat`*
	
	```
	Set WshShell = CreateObject("WScript.Shell") 
	WshShell.Run chr(34) & "E:\Automation\obsidian-sync.bat" & Chr(34), 0
	Set WshShell = Nothing
	```

8. Lastly, configure a **task scheduler**, on **Windows**
	- Press `win+r`
	- Open `taskschd.msc`
	- On the **top left**, click **action** then **create a task**, [[#Create Task Example|like this]]
	- The main things is, we need to make a task schedule every **hour** to run our **silent-sync.vbs**, that will run our **obsidian-sync.bat** on **background** **silently**, while excluding all file in **fileObsidianIgnore.txt**

9. **F I N I S H !**

**Congratulations !**
**Btw, to check if your batch file is working or not,** 
**you could execute the .bat file or .vbs file, by clicking it**

**-- Good Luck  !**














## Screenshot :
### Default Configuration Folder directory
![[Pasted image 20230827015543.png]]
![[Pasted image 20230827015819.png|600]]
![[Pasted image 20230827015803.png|600]]
![[Pasted image 20230827015857.png|600]]

### Create Task Example
![[Pasted image 20230827023408.png]]
![[Screenshot 2023-08-27 023455.png|600]]
![[Pasted image 20230827023638.png|600]]
![[Pasted image 20230827023856.png|600]]
![[Pasted image 20230827023712.png|600]]