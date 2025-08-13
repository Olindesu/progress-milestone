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

After done with the installation of Open5GS on the 5G Cores VM, I have start with the configuration of 5G cores to build a 5G SA network that can be connected from others VM. On default, the configuration of 5G Cores provided by Open5GS will all be set in loopback address. So we will need to apply a Host-only Adapter on the VM to obtain a new VM IP that can be communicate between VMs, the ip are automatically assigned are 192.168.56.XXX . Here are the table for each of the IP assigned on the VMs.

| Virtual Machines No. | Representing | IP Address |
| --- | --- | --- |
| VM1 | Open5GS Core | 192.168.56.101 |
| VM2 | UERANSIM gNB | 192.168.56.104 |
| VM3 | UERANSIM UE | 192.168.56.102 |

To setup a 5G SA Network, there are som
