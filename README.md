# Monitoring ICMP Traffic with Wireshark & Security Protocols on Azure

## Step 1: Access Azure Portal
- Go to [https://portal.azure.com](https://portal.azure.com)
- Confirm your Windows and Ubuntu VMs are running

## Step 2: Launch Microsoft Remote Desktop (macOS)
- Install Microsoft Remote Desktop from the App Store (if not already installed)
- Connect to your Windows 10 VM

## Step 3: Download and Install Wireshark
- Visit [https://www.wireshark.org/download.html](https://www.wireshark.org/download.html)
- Download the 64-bit Windows installer

![Step 3](images/ScreenshotICMP1.png)

## Step 4: Complete Wireshark Setup
- Run the installer
- Accept licenses and allow Npcap installation
- Skip USBcap installation

![Step 4](images/ScreenshotICMP2.png)

## Step 5: Launch Wireshark
- Open Wireshark and select the Ethernet interface
- Begin capturing traffic

## Step 6: Observe Traffic
- Initially, you’ll see lots of traffic before applying any filters

## Step 7: Filter for ICMP Traffic
- In the display filter bar, type `icmp` to isolate ping traffic

![Step 7](images/ScreenshotICMP5.png)

## Step 8: Open PowerShell as Admin
- Right-click → “Open PowerShell (Admin)”

![Step 8](images/ScreenshotICMP6.png)

## Step 9: Copy Ubuntu VM's Private IP from Azure
- Go to your Ubuntu VM in Azure and copy its private IP address

## Step 10: Run Ping Command in PowerShell
- In PowerShell, type:
  ```powershell
  ping 10.0.0.5
  ```
- Return to Wireshark to observe filtered ICMP traffic

![Step 10a](images/ScreenshotICMP8.png)  
![Step 10b](images/ScreenshotICMP9.png)

## Step 11: Start Continuous Ping
- In PowerShell:
  ```powershell
  ping 10.0.0.5 -t
  ```

![Step 11](images/ScreenshotICMP10.png)

## Step 12: Observe ICMP Requests and Replies
- Wireshark should show a stream of Echo requests and Echo replies

## Step 13: Apply NSG Rule to Block ICMP
- Go to the Ubuntu VM’s Network Security Group
- Add a new **Inbound rule**:
  - Protocol: `ICMPv4`
  - Action: `Deny`
  - Priority: Lower than existing allow rules

![Step 13](images/ScreenshotICMP11.png)

## Step 14: Observe Ping Timeouts
- Ping should now display: `Request timed out`
- Wireshark shows ICMP requests but **no replies**

![Step 14](images/ScreenshotICMP12.png)

## Step 15: Remove/Disable the ICMP Deny Rule
- Go back to the NSG
- Remove or disable the deny rule

## Step 16: Confirm Ping Recovery
- You should start seeing ICMP replies again in Wireshark

![Step 16](images/ScreenshotICMP13.png)
