# run-tracker
Dead simple run tracker, saves data locally/personal icloud. No ads, no tracking. Single html page. For people that dont need fancy features or that dont want to download an app just to do basic things.

## Usage:
Hosted on github pages: https://farisjarrah.github.io/run-tracker/
Or just save and open the index.html in a your browser locally.
Even on github pages, no data is transmitted anywhere and you are only opening up your data json file from local/cloud storage of your choice.

If you save your file to icloud storage you can use the same file for all of your devices, probably works with any other filesystem integration type too (like google drive), but untested.

### Saving on iPhone/iPad (iOS Safari)
Tap **Save file** and choose **Save to Files**. iOS Safari can't overwrite the file you opened, so treat the save as a *backup copy*: if iOS offers it, pick **Replace** to keep a single file; if it saves a new file instead (e.g. `run-data.json 2`), that's fine too — once you have a backup, you can delete the older duplicate.

**You can't lose progress either way.** As you track, the app auto-saves your latest session on the device (in the browser's storage). If you refresh or the page closes, the load screen shows **Restore your last session** and brings everything right back. The file you save to iCloud is your cross-device backup. To make a file show up in the Files picker, open it once in the Files app so iCloud downloads it, then always pick **Browse → iCloud Drive**, not "Recents".

### Install as an app (PWA)
Works as a normal website, or install it to your home screen for a full app experience:
- **iPhone/iPad:** Share button → **Add to Home Screen**.
- **Android:** Chrome menu (⋮) → **Add to Home screen** or **Install app**.
- **Desktop:** install icon in the Chrome/Edge address bar (or menu → **Install...**).
The installed app opens full-screen with its own icon; the data flow (open JSON + auto-save + save to iCloud) is unchanged.

## WARNING:
This was vibecoded using the free OpenCode Big Pickle AI Model 09/2026.

## Credits
Icon: "Running shoe" by Delapouite, from game-icons.net (CC BY 3.0).

## Screenshots
<img width="741" height="929" alt="image" src="https://github.com/user-attachments/assets/587a0f9b-2590-48dd-982b-86ec2d1c0190" />
<img width="682" height="974" alt="image" src="https://github.com/user-attachments/assets/36125795-a32c-4ae4-bdcf-fcb11f7fc600" />
<img width="670" height="898" alt="image" src="https://github.com/user-attachments/assets/054349ab-0b95-4042-86b0-ee8b3d284387" />
<img width="666" height="810" alt="image" src="https://github.com/user-attachments/assets/ecf72fd8-03c6-4337-9656-7d285219fa74" />
<img width="660" height="961" alt="image" src="https://github.com/user-attachments/assets/78e43cc6-36c8-47bb-a809-a51088935c35" />
