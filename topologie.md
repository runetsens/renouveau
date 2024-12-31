# topologie routage avec debian

![Capture d’écran du 2024-12-31 00-35-52](https://github.com/user-attachments/assets/f0884cc0-6815-4d93-9e6d-5eb49ff8a1e1)

# ping de la machine 10 aux machines des reseaux 10.0.1.0 et 10.0.2.0
![Capture d’écran du 2024-12-31 12-22-57](https://github.com/user-attachments/assets/869d394b-d4d5-4747-9c29-de1ffd2243a9)

# ping de la machine 12 aux machines des reseaux 10.0.0.0 et 10.0.2.0
![Capture d’écran du 2024-12-31 12-23-45](https://github.com/user-attachments/assets/f95e3b8f-8a6a-4e99-9dec-6f7bc73826fb)

# ping de la machine 13 aux machines des reseaux 10.0.0.0 et 10.0.1.0
![Capture d’écran du 2024-12-31 12-31-29](https://github.com/user-attachments/assets/f90b367d-ddf4-4309-9314-e7db4735bf1e)


# configuration

## Machine 10 (Client)

    IP : 10.0.0.10/24
    Passerelle par défaut : 10.0.0.1
    command: ip 10.0.0.10 10.0.0.1
    
## Machine 11 (Client)

    IP : 10.0.0.11/24
    Passerelle par défaut : 10.0.0.1
    command: ip 10.0.0.11 10.0.0.1
    
## Machine 12 (Client)

    IP : 10.0.1.12/24
    Passerelle par défaut : 10.0.1.1
    command: ip 10.0.1.12 10.0.1.1
    
## Machine 13 (Client)

    IP : 10.0.2.13/24
    Passerelle par défaut : 10.0.2.1
    command: ip 10.0.2.13 10.0.2.1
    
## Routeur R0

### Activer le routage IP :

sudo sysctl -w net.ipv4.ip_forward=1

### Configurer les interfaces réseau :

sudo ip address add 10.0.0.1/24 dev ens4
sudo ip address add 192.168.0.250/24 dev ens5

### Ajouter les routes statiques :

sudo ip route add 10.0.1.0/24 via 192.168.0.251 dev ens5
sudo ip route add 10.0.2.0/24 via 192.168.0.252 dev ens5

## Routeur R1

### Activer le routage IP :

sudo sysctl -w net.ipv4.ip_forward=1

### Configurer les interfaces réseau :

sudo ip address add 192.168.0.251/24 dev ens4
sudo ip address add 10.0.1.1/24 dev ens5

### Ajouter les routes statiques :

sudo ip route add 10.0.0.0/24 via 192.168.0.250 dev ens4
sudo ip route add 10.0.2.0/24 via 192.168.0.252 dev ens4

## Routeur R2

### Activer le routage IP :

sudo sysctl -w net.ipv4.ip_forward=1

### Configurer les interfaces réseau :

sudo ip address add 192.168.0.252/24 dev ens4
sudo ip address add 10.0.2.1/24 dev ens5

### Ajouter les routes statiques :

sudo ip route add 10.0.0.0/24 via 192.168.0.250 dev ens4
sudo ip route add 10.0.1.0/24 via 192.168.0.251 dev ens4



    
# installation 
 
 Preconfigured Appliances

    "Debian (GNS3 Appliance)"
        A lightweight Debian-based appliance specifically prepared for GNS3.
        Found in the GNS3 Marketplace.
        Suitable for routing and basic network functions.
        
 ![Capture d’écran du 2024-12-30 15-30-18](https://github.com/user-attachments/assets/d3b43086-ca5c-4eb1-9958-4167c68690ec)

The message "Missing files" indicates that GNS3 cannot find the required disk images for the selected Debian appliance. Here's how you can resolve this issue:

---

### **Steps to Download and Import the Required Files**

1. **Select a Debian Version**:
   - Choose the version of Debian you want to use (e.g., Debian version 12.6 or 11.8).
   - You can proceed with the latest or most stable version.

2. **Download the Missing Files**:
   - Click on the "Appliance Info" button or visit the GNS3 Marketplace to check the required file links.
   - Alternatively, download the `.qcow2` file directly from the suggested URL (usually linked in the appliance's documentation or the GNS3 Marketplace).

3. **Place the File in the Correct Directory**:
   - Save the downloaded `.qcow2` file to a directory that GNS3 can access.
   - By default, this is the `~/GNS3/images` directory on Linux or the equivalent directory on Windows/Mac.

4. **Refresh the Appliance Installation**:
   - After placing the file in the appropriate directory, return to the GNS3 appliance setup wizard and click "Refresh."
   - The "Missing files" status should change to "Ready."

5. **Complete the Installation**:
   - Once the file is found, click "Next" and follow the wizard to finalize the installation.

---

### **Where to Download the Files**
- Visit the GNS3 Marketplace page for the Debian appliance: [GNS3 Marketplace Debian Appliance](https://gns3.com/marketplace/appliances).
- Look for the download link corresponding to the `.qcow2` image file for the selected Debian version.
- Ensure you download from a reliable source to avoid corrupted or incompatible files.

---

### **Tips**
- **If you already have a Debian ISO:** You can create a custom QEMU VM in GNS3 using the ISO to install Debian manually.
- Ensure your internet connection is stable during the download process.
- If you are unable to download the file automatically, manually download the `.qcow2` file and specify its location during the appliance setup.

Let me know if you need further assistance! 😊
