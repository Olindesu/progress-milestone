# Introduction

The project I been doing is developing a Network Digital Twin (NDT). NDT is a virtual clone of a physical network that capturing its architecture, components, and real-time state. NDT is important as it provide a safe, risk-free environment for simulating, analyzing, and optimizing network performance before making change on the live network.

# Objective

The objective of this project is to develop a 5G Standalone (5G SA) NDT for a physical network to help with simulate, analyze, and optimize network performance with the implementation of the AI models and apply the simulated result back to live network to help with improvement of the 5G SA Network.

# Milestones
## Milestone 1

### Preparation

Before starting with the development of NDT, I have first created 3 virtual machines (VMs) using Oracle VirtualBox and downloaded Ubuntu 22.04.5 (Jammy Jellyfish) from Ubuntu website to separate the 5G Cores, the Radio Access Network(RAN) using Next Generation Nobe B (gNB), and the User Equipment (UE). For the 5G Cores, I have selected Open5GS, an open-source project for building a personal NR/LTE network, and for the gNB and UE, I have selected UERANSIM, an open source state-of-the-art 5G UE and RAN simulator.

### Installation

For the installation of Ubuntu Jammy, you can [click here](https://releases.ubuntu.com/jammy) for the installation step of Ubuntu Jammy.\
For the installation of Open5GS, you can [click here](https://open5gs.org/open5gs/docs/guide/01-quickstart) for the installation step of Open5GS.\
For the installation of UERANSIM, you can [click here](https://github.com/aligungr/UERANSIM/wiki/Installation) for the installation step of UERANSIM.

### Configuration

### Open5GS

After done with the installation of Open5GS on the 5G Cores VM, I have start with the configuration of 5G cores to build a 5G SA network that can be connected from others VM. On default, the configuration of 5G Cores provided by Open5GS will all be set in loopback address. So we will need to apply a Host-only Adapter on the VM to obtain a new VM IP that can be communicate between VMs, the ip are automatically assigned are 192.168.56.XXX . Here are the table for each of the IP assigned on the VMs, you can check the IP address using 'ip a' and look for the 'inet' inside 'enp0s8'.

| Virtual Machines No. | Representing | IP Address |
| --- | --- | --- |
| VM1 | Open5GS Core | 192.168.56.101 |
| VM2 | UERANSIM gNB | 192.168.56.104 |
| VM3 | UERANSIM UE | 192.168.56.102 |

To setup a 5G SA Network, there are some necessary network functions (NFs) need to set up, mainly are NRF, AMF, SMF, AUSF, UDM, UDR, PCF, and UPF. Each of them are required to bind on the Open5GS VM IP with unique port. Here are the table for each of the NFs and their port assigned:

| Network Functions | IP Address | Assigned Port |
| --- | --- | --- |
| NRF | 192.168.56.101 | 7777 |
| AMF | 192.168.56.101 | 7778 |
| SMF | 192.168.56.101 | 7779 |
| AUSF | 192.168.56.101 | 7780 |
| UDM | 192.168.56.101 | 7781 |
| UDR | 192.168.56.101 | 7782 |
| PCF | 192.168.56.101 | 7783 |
| UPF | 192.168.56.101 | 9090 |

Apart from giving them unique port, it is also important to make change inside their own .yaml file if there is existence of other NFs inside it to ensure there is communication between it.

### UERANSIM gNB

After we have done with the configuration of 5G SA NFs, we will move to our RAN, it is very simple to configure the RAN using the preset configuration file given when installing UERANSIM, all it need is just some simple changes:

```
--- mcc: '999'          # Mobile Country Code value
--- mnc: '70'           # Mobile Network Code value (2 or 3 digits)
+++ mcc: '001'
+++ mnc: '01'

nci: '0x000000010'  # NR Cell Identity (36-bit)
idLength: 32        # NR gNB ID length in bits [22...32]
tac: 1              # Tracking Area Code

--- linkIp: 127.0.0.1   # gNB's local IP address for Radio Link Simulation (Usually same with local IP)
--- ngapIp: 127.0.0.1   # gNB's local IP address for N2 Interface (Usually same with local IP)
--- gtpIp: 127.0.0.1    # gNB's local IP address for N3 Interface (Usually same with local IP)
+++ linkIp: 192.168.56.104
+++ ngapIp: 192.168.56.104
+++ gtpIP: 192.168.56.104

# List of AMF address information
amfConfigs:
---  - address: 127.0.0.5
+++ - address: 192.168.56.101
    port: 38412

# List of supported S-NSSAIs by this gNB
slices:
  - sst: 1

# Indicates whether or not SCTP stream number errors should be ignored.
ignoreStreamIds: true
```

All it need to do is make change on mcc and mnc, it is ok to use the default setting '999' and '70' but '001' and '01' is the generally used for testing, remember if you updated these 2 values, you will also need to update it on the Open5GS NFs if its .yaml configuration have mentioned mcc and mnc. Then, update linkIp, ngapIp, gtpIp to the device own assigned IP Address since default is on loopback addresses. Lastly, make change on the address inside 'amfConfigs' to match with the VM IP that AMF is located. After doing all of that, the configuration of gNB is completed.

### UERANSIM UE

Moving on from gNB, you will need to create a UE to simulate connection, similar 
