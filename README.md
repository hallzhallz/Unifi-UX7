# Unifi-UX7
Securing a Ubiquiti Unifi UX7 for Home Office Use

[Browse recommended products on amazon](https://amzn.to/4diDgac)

## Preparation

#### Factory Reset
Hold the reset pin in for ~10 seconds a factory reset message should display

#### Unfi App (Android/iOS)
Install the `Unifi` app on your phone or tablet.

## Adoption

1. Plug in the Router and wait for it to boot.
2. Open the `Unifi` App and when the router appears click setup.
3. When prompted choose to setup a new network/site.
4. Enter the site name and wifi name and password.

## Networks

### Login
1. Login to `Unifi Site Manager` https://unifi.ui.com
2. From Site manager click `Control Plane` under the device or cick the device name, then click the cog in the bottom left of the screen/menu.

### Networks/VLANs

1. Click Networks left menu item
2. Click New Virtual network and enter the details (Create Main VLAN)
   |   |    |
   | -- | -- |
   | Name | Main |
   | Auto-Scale Network | De-select |
	 | IPv4 Address | 192.168.10.1/24 |
   | VLAN ID | 100 |
3. Click `Create`.
4. Click New Virtual network and enter the details (Create IoT VLAN)
   |   |    |
   | -- | -- |
   | Name | IoT |
   | Auto-Scale Network | De-select  |
   | IPv4 Address | 192.168.101.1/24 |
   | VLAN ID | 101 |
6. Click `Create`.

### Zones (VLAN Firewal Rules)

1. Click `Traffic and Firewall Rules` under `Policy Engine` menu heading.
2. Under `Upgrade to Zone-Based Firewall` click `Click to upgrade`.
3. Under `Zones` click `Create Zone` and enter details
   |   |   |
   | -- | -- |
   | Zone Name | IoT |
   | Networks / Interfaces > IoT | Check and Save |
4. Click `Add Entry`.
5. In the `Zone Matrix` click `All Policies (…)`.
6. Then at the bottom of the policies click `Create Policy` and enter the details (Internal to IoT):
   |   |    |
   | -- | -- |
   | Name | Allow Internal to IoT |
   | Source Zone | Internal |
   | Action | Allow |
   | Auto Allow Return Traffic | Checked/On |
   | Destination Zone | IoT |
7. Click Add Policy
8. Then at the bottom of the policies click `Create Policy` and enter the details (VPN to IoT):
   |  |  |
   | -- | -- |
   | Name | Allow VPN to IoT |
   | Source Zone | VPN |
   | Action | Allow |
   | Auto Allow Return Traffic | Checked/On |
   | Destination Zone | IoT |
9. Click Add Policy
10. Then at the bottom of the policies click `Create Policy` and enter the details (Block IoT from accessing router management features):
    |  |  |
    | -- | -- |
    | Name | Block IoT to Gateway Remote Management |
    | Source Zone | IoT |
    | Action | Block |
    | Destination Zone | Gateway |
    | Port | List |
11. In `Port List` click `Create New` and enter the details
    |  |  |
    | -- | -- |
    | Name | Ubiquiti Router Remote Management |
    | Ports | `80`,`443`,`22`,`5349`,`8443` |
12. Click `Create`.
13. Click `Add Policy`.

### Ports
Since the UX7 only has one LAN port it must always act as a Trunk Port and can only be native to the Default VLAN 1 ( This is a device limitation).

1. Click the `Overview` left hand menu item
   1. Under `Ethernet Port Profiles` click `Create New` and enter:
      |  |  |
      | -- | -- |
      | Name | Switch Trunk Port |
      | Port | Active |
      | Native  VLAN | Default (1) |
      | Tagged VLAN Management | Allow All |
   2. Then Click `Apply Changes` at bottom of screen.
3. Select `Ports` Icon from left menu near top
4. Click the `LAN Port` (Port 1)
5. Under `Advanced` switch to `Manual`.
6. Check `Ethernet Port Profile`.
7. Select the `Switch Trunk Port Profile`.
8. Click `Apply Changes`.


