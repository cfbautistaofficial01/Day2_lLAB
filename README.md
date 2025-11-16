# Day 2 Afternoon Lab: The Wireshark Capture

**Your Goal:**
You will use Wireshark to capture, filter, and analyze **unencrypted** network traffic in plain text. This lab will prove why insecure (HTTP) websites are dangerous.

**Your Tools:**
* A Windows 11 PC
* Wireshark 4.2.6 (with Npcap 1.79)
* A web browser

**Your Target:**
We will be using a website that is *intentionally* insecure for training purposes:
* **`http://neverssl.com/`**

---

### **Part 1: Installation & Setup**

If you have not already, you must install Wireshark.

1.  Open your browser and go to: **`https://www.wireshark.org/download.html`**
2.  Download the **"Windows Installer (64-bit)"** for version 4.2.6.
3.  Run the installer. Click "Next" on the first few screens.
4.  **CRITICAL STEP:** The installer will ask to install **"Npcap"**. You **MUST** install this.
5.  When you get to the Npcap options screen with three checkboxes:
    * **UNCHECK** "Restrict Npcap driver's access..."
    * **UNCHECK** "Support raw 802.11 traffic..." (We are on a LAN cable)
    * **UNCHECK** "Install Npcap in WinPcap API-compatible mode"
6.  Finish the installation and open the Wireshark application.

---

### **Part 2: The Capture (Step-by-Step)**

1.  **Open Wireshark.** You will see the Welcome Screen.
2.  Find your **"Ethernet"** connection (it will have a live graph next to it).
3.  **Double-click "Ethernet"** to start the capture. You will see packets flooding the screen. This is normal!
4.  Open your web browser.
5.  **Carefully type** the *exact* address. It **must** be `http://` (not https):
    * **`http://neverssl.com/`**
6.  Wait for the simple, one-page site to load.
7.  Go back to your Wireshark window.
8.  Click the **red "Stop" button** (top-left) to stop the capture.

---

### **Part 3: The Analysis (What to Find and Log)**

Now you have thousands of packets. Let's find the ones we care about using the **Display Filter** bar.

#### Finding #1: The "Phonebook" Lookup (DNS)

* **Action:** In the filter bar, type **`dns`** and press Enter.
* **What to Look For:** Find the packet that says **"Standard query... A neverssl.com"** in the "Info" column.
* **What This Means (Definition):**
    * **`DNS` (Domain Name System):** This is the "internet's phonebook."
    * **What you found:** This packet is your PC (the *Source*) asking the DNS server (the *Destination*), "What is the IP address for `neverssl.com`?"

#### Finding #2: The "Webpage Order" (HTTP)

* **Action:** Click the "X" in the filter bar to clear your `dns` filter.
* **Action:** Now, type **`http`** and press Enter.
* **What to Look For:** Find the packet that says **`GET / HTTP/1.1`**. (This is you asking for the main `/` page of the site).
* **What This Means (Definition):**
    * **`HTTP` (Hypertext Transfer Protocol):** This is the *unencrypted language* of the web.
    * **`GET` Request:** This is your browser (the "Customer") "ordering" the webpage from the server (the "Kitchen").

#### Finding #3: The "A-ha!" Moment (Plain Text)

* **Action:** **Right-click** on the **`GET / HTTP/1.1`** packet.
* **Action:** Go to **Follow > TCP Stream**.
* **What to Look For:** A new window will open showing the *entire conversation* in plain text.
    * **Red Text:** This is the data *your* PC sent.
    * **Blue Text:** This is the data the *server* sent back.
* **What This Means (Definition):**
    * **`TCP Stream`:** This shows the raw, complete conversation. Because it's `HTTP`, it is in **Plain Text**.
    * **Result:** You can read the raw HTML of the webpage! This proves that anyone "listening" (like us) can see *everything* that is happening on an insecure website.

 
