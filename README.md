<h1>Configuring and Troubleshooting DNS</h1>

This project demonstrates the configuration and troubleshooting of DNS using a Domain Controller (DC) as a DNS server in a cloud-based environment. Key highlights include...

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop Connection
- DNS Manager

<h2>Operating Systems Used</h2>

- Windows Server 2022
- Windows 11 Pro, version 25H2

<h2>Table of Contents</h2>

<h2>DNS A Record Creation and Troubleshooting</h2>

<p>
This environment consists of two virtual machines (VMs) within a unified virtual network. A Windows Server 2022 VM serves as the DNS server (DC-1), while a secondary VM running Windows 11 Pro serves as the client (Client-1). Client-1 is joined to DC-1's domain and routes all DNS traffic through DC-1. The configuration looks like this:
</p>
<br />

<p>
<img width="750" height="650" alt="image" src="https://github.com/user-attachments/assets/7f46d88d-3412-4b20-8e49-ab4cd5ddc66f" />
</p>

<p>
DC-1 is configured with a static IP address. This prevents connectivity failures that would occur if the DNS server's IP address changed via DHCP renewal. 
  
It's important to note that failures may not happen instantly for every resource. If a client recently contacted a domain, its IP address may still be in its local cache. The domain will continue to work until the Time to Live (TTL) expires.
</p>

<p>
<img width="750" height="650" alt="image" src="https://github.com/user-attachments/assets/7fae1038-708a-49e2-840e-71ab8a8833f8" />
</p>

<p>
Log in to Client-1 as the domain admin account (MYDOMAIN\example_admin), then run "ping DC1DNS" and "nslookup DC1DNS" in an elevated Command Prompt. Both of these commands fail because there is no DNS record in the local DNS cache, HOSTS file, or the DNS server that maps the "DC1DNS" hostname to an IP address.
</p>
<br />

<p>
<img width="750" height="650" alt="image" src="https://github.com/user-attachments/assets/f04d7e73-c23c-4d63-bfca-65ea0a862d3c" />
</p>

<p>
From DC-1, open DNS Manager and create an A record for the hostname "DC1DNS" that maps to DC-1's private IP address (10.0.0.4). Return to Client-1, and run "ping DC1DNS". Client-1 is now receiving a reply from 10.0.0.4 (DC-1).
</p>
<br />

<p>
<img width="750" height="393" alt="image" src="https://github.com/user-attachments/assets/a816a72c-6f4b-465f-9844-0b22f3bca550" />
</p>

<p>
<img width="750" height="650" alt="image" src="https://github.com/user-attachments/assets/b63d35f5-8887-41a2-9b7e-71a56026a609" />
</p>

<h2>Local DNS Cache Troubleshooting</h2>

<p>
From DC-1, change the "DC1DNS" record's IP address to "8.8.8.8". Return to Client-1, and run "ping DC1DNS". Client-1 is still receiving a reply from "10.0.0.4", despite changing the A record's IP address to "8.8.8.8". This is because the client always checks its local DNS cache first to prioritize speed before querying the DNS server, and the client has not yet updated its DNS cache (TTL has not expired) to reflect the changes made to the A record.
</p>
<br />

<p>
<img width="685" height="394" alt="image" src="https://github.com/user-attachments/assets/fcb30c3c-71b7-4f63-b444-c69ed9f3d28d" />
</p>

<p>
<img width="750" height="650" alt="image" src="https://github.com/user-attachments/assets/6a861178-fe7b-4271-91be-429eb8884b9f" />
</p>

<p>
Observe the DNS cache by running "ipconfig /displaydns", which shows the entry for "DC-1.mydomain.com". Flush the DNS cache by running "ipconfig /flushdns", then run "ipconfig /displaydns" to confirm the cache is empty.

Run "ping DC1DNS" again, and observe that Client-1 is now receiving a reply from 8.8.8.8. Since Client-1's DNS cache has been flushed, Client-1 contacts the DNS server to retrieve the new IP address for the "DC1DNS" A record.
</p>
<br />

<p>
<img width="750" height="650" alt="image" src="https://github.com/user-attachments/assets/27ffde25-4fa5-4015-81f6-e4b7733bdb40" />
</p>

<p>
<img width="750" height="650" alt="image" src="https://github.com/user-attachments/assets/466136b0-1b4c-46a7-97c7-50cf19083831" />
</p>

<h2>CNAME Records</h2>

<p>
From DC-1, 
</p>
