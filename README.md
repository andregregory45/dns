<h1>Configuring and Troubleshooting DNS</h1>

This project demonstrates the configuration and troubleshooting of DNS using a Domain Controller (DC) as a DNS server in a cloud-based environment. Key highlights include

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop Connection
- 

<h2>Operating Systems Used</h2>

- Windows Server 2022
- Windows 11 Pro, version 25H2

<h2>Table of Contents</h2>

<h2>DNS A Record Creation</h2>

<p>
This environment consists of two virtual machines (VMs) within a unified virtual network. A Windows Server 2022 VM serves as the DNS server (DC-1), while a secondary VM running Windows 11 Pro serves as the client (Client-1). Client-1 is joined to DC-1's domain and routes all DNS traffic through DC-1. The configuration looks like this:
</p>
<br />

<p>
<img width="750" height="650" alt="image" src="https://github.com/user-attachments/assets/7f46d88d-3412-4b20-8e49-ab4cd5ddc66f" />
</p>

<p>
Log in to Client-1 as the domain admin account (MYDOMAIN\example_admin), then run "ping DC1DNS" and "nslookup DC1DNS" as an administrator. Both of these commands fail because there is no DNS record in the local DNS cache, HOSTS file, or the DNS server that maps the "DC1DNS" hostname to an IP address.
</p>
<br />

<p>
<img width="750" height="650" alt="image" src="https://github.com/user-attachments/assets/f04d7e73-c23c-4d63-bfca-65ea0a862d3c" />
</p>
