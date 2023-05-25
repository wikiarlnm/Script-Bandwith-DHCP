Bandwidth configuration automatically uses a script specified with the MAC address from the DHCP client

:local queueName "Client- $leaseActMAC";
:if ($leaseBound = "1") do={
    /queue simple add name=$queueName target=($leaseActIP . "/32") limit-at=40M/40M max-limit=50M/50M comment=[/ip dhcp-server lease get [find where active-mac-address=$leaseActMAC && active-address=$leaseActIP] host-name];
} else={
    /queue simple remove $queueName
}
