## Docker Container Security...

Next...<br>
So, I think what I want to learn from building containers is, when building these containers - I realized that I am, in a way building a custom Linux operating system (and I like me some Linux). My idea is: let's say you start a new position at work or remote work. As you start work with company A - you use a preconfigured docker container. Now, I have no idea if this is logical in the real world, so there that...
This Docker Container is preconfigured to only allow internet traffic from your use - all other incoming traffic is blocked (verify my statement is correct) The user can download if necessary (This does cause an issue).
I want to also allow the user of the container to be able to download and set up the container however they want in regards to their work. So, I still have more reading to do...

I want to create a *safe* & *secure* `container`

I will start with what might be consider best practices.<br>
I will attempt to allow for further augmentations if further research was conducted.<br>

### Use a `Non-Root` User <br>

1. Modify the `Dockerfile` to avoid running as `root`:

```
# Add a new user
RUN useradd -m appuser

# Switch to non-root user
USER appuser
```

2. Reduce Container Privileges:

    - You want to limit the privilages, enter the following: <br>

` docker run --read-only --memory=128 --cpus=0.5 mydockerapp ` <br>

- `--read-only`  -> Makes the files system immutable. <br>
- `--memory=128` -> Limits memory usage. <br>
- `--cpu=0.5`    -> Limits CPU consumption. <br>

3. `Enable` Logging for *Unauthorized* Access

...Thoughts...
ok, so this is where `Docker` kind of blows my mind. I want to create a secure platform, which is what the `Docker Container` can achieve. But because it is Linux-based, I can implement security bash scripts and custom firewalls.?. Think about all the prevention tools!!! YO!<br>

This is going to be fun!!! <br>

` New-Item -Path . -Name "monitor.sh" -ItemType "file" ` <br>

### Improving the Base Image

In the *original* docker file I started with regular `python 3.12`. To make a more secure `Contianer`, I found that there are many other python docker versions - these different versions have different preconfigured security features.<br>

Changing from: `python 3.12` to `python 3.12-alpine`<br>

The reason I am choosing `Alpine Linux` is because it claims that it reduces the attack surface. <br>

- `apk add --no-cache` this installs *only* the essential dependencies. <br>
- `( rm -rf /var/cache/apk/* )` Removes unneeded package caches.<br>

### Preventing Common Exploits


1. Harden the Runtime Security <br>
- Run the container with restrictive security options: <br>

``` docker run --read-only --tmpfs /tmp --cap-drop=ALL --cap-add=NET_RAW --security-opt no-new-privileges secure-container``` <br>

.!.Ok.!. but what does all that mean?<br>

`--read-only` --> Makes the *entire* filesystem immutable. <br>
`--tmpfs /tmp` --> Allows only *temporary* files in memory. <br>
`--cap-drop=ALL` --> *Removes* all unnecessary Linux capabilities. <br>
`--cap-add=NET_RAW` --> *Allows* `ICMP` (ping) monitoring. <br>
`--security-opt no-new-privileges` --> *Prevents* `privilege escalation`. <br>

### Enhancing Logging for Security Monitoring

In this section, the idea is to have a log-driven container that `logs` *all* important security events.<br>

We are going to *add to* the `monitor.sh`. <br>

In `monitor.sh` there is already a script to log `unauthorized` pings (ICMP packets) on line `~#27`.<br>

For starters, lets *log* `SSH` and `system access attempts`<br>

`tail -F /var/log/auth.log | tee -a /var/log/security/auth_logs.txt &` <br>
`tail -F /var/log/auth.log | tee -a /var/log/security/syslog.txt &` <br>

- The `ICMP` pings captures network intrusion attempts. <br>
- The `SSH` logs unauthorized access attempts.<br>


### Isolate the Container Using a Secure Network

By creating a *seperate* *internal network* (`secure-net`). The container now can only acess the internet, but not other containers or internal services.<br>

Enter this command:<br>

```
docker network create --internal secure-net
docker run --network=secure-net secure-container
```
That command:<br>
- Blocks direct access from other containers.
- Allows safe web browsing.
- Prevents attackers from laterally moving inside the system.


### Restrict Internet Access with a Firewall

I don't want to completely isolate the container, starting the `firewall` with this configuration:

```

- Flush exisitng rules to start fresh                        --> Start fresh                                          --> iptables -F
- Set the *default policy:* Drop all traffic by default.     --> Blocks all incoming connections                      --> iptables -P INPUT DROP, iptables -P FORWARD DROP, iptables -P OUTPUT DROP
- Allow outbound traffic for web browsing (HHTP & HTTPS)     --> Allows only web browsing and downloading             --> iptables -A OUTPUT -p tcp --dport 80 -j ACCEPT # HTTP, iptables -A OUTPUT -                                                                                                                         p tcp --dport 443 -j ACCEPT # HTTPS
- Allows DNS resolution (required for domain-based browsing) --> Prevent *malware* from connecting to malicious sites --> iptables -A OUTPUT -p udp --dport 53 -j ACCEPT
- Allows loopback communication for some processes           --> Ensures system services work (DNS, loopback)         --> iptables -A INPUT -i lo -j ACCEPT, iptables -A OUTPUT -o lo -j ACCEPT

```

### Use a `Proxy Server` for Extra Control

So, before we can go on *quick!* what is a proxy server?<br>


















