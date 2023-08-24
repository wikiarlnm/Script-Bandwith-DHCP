Automatically configuring bandwidth involves utilizing a script associated with the MAC address from the DHCP client.

Here's the script that can be used:

:local queueName "Client- $leaseActMAC";
:if ($leaseBound = "1") do={
    /queue simple add name=$queueName target=($leaseActIP . "/32") queue=pcq_upload/pcq_download comment=[/ip dhcp-server lease get [find where active-mac-address=$leaseActMAC && active-address=$leaseActIP] host-name];
} else={
    /queue simple remove $queueName
}

However, before implementing the DHCP lease script, you need to create a PCQ type queue for both uploads and downloads. As you mentioned earlier, here's an example of how to add these queues:

Open your Winbox terminal.
Paste and run the following commands:

/queue type
add kind=pcq name=pcq_download pcq-rate=500M pcq-classifier=dst-address
add kind=pcq name=pcq_upload pcq-rate=500M pcq-classifier=src-address

By doing this, you'll be able to ensure that the bandwidth allocation is properly managed for your DHCP clients based on their MAC addresses.
