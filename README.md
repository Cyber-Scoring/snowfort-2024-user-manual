# Snowfort 2024: User Manual

## Overview

**Who?** 40 teams globally, open-entry  
**When?** March 16th, 2024, 11am to 7pm EDT  
**Where?** Online

Snowfort 2024 is a **defensive** cybersecurity competition where **40 teams across the globe** are given **five services with five vulnerabilities each**.

Website: https://snowfort2024.cyberscoring.ca/
Discord: https://discord.com/invite/RNKB2ekFXY

## Registration

**Registration closes on March 13th at 11:59pm EDT.**

Teams can register here: https://forms.gle/zrCkMAao12SMXYKk8
Individuals can register here and will be placed on a team: https://forms.gle/vQ97BUyxmKs8LEYy6

Due to limited infrastructure, only 40 teams can register.

Initially registration was restricted to Canadian teams only, but this restricted has since been lifted.

## Schedule

**March 13:**
11:59pm EDT - Registration closes.

**March 14:**
Team channels will be made in the Discord server.

**March 15**:
VPN configs will be distributed to the teams via their Discord channels. VPN infrastructure will be put online for teams to test their connections.

**March 16:**
10:00am EDT - SSH keys will be distributed.
11:00am EDT - Competition starts!
7:00pm EDT - Competition ends.

## Prizes

Prizes are restricted to Canadian teams only.

**1st Place:**
$500 CAD (Prepaid VISA Gift Cards)
x5 [Bulletproof](https://bulletproofsi.com/) Hoodies

**Best Writeups**:
x2 prizes for $50 CAD each

Thanks to [Bulletproof](https://bulletproofsi.com/) for sponsoring the 1st place prize.

## Scoring

The competition will run for 480 game ticks where each tick is 60 seconds (total of 8 hours). Each game tick, an attack bot will perform 5 malicious and 5 benign request sequences on each of your services.

Benign requests test standard user functionality. Malicious requests test vulnerabilities. One benign request and one malicious request make a request pair. Each request pair is tied to one vulnerability. The requests being tested may change throughout the competition. For example, request pair 4 may begin being tested after tick 200.

Each request may either result in a success, failure, or down state. Success is when the request worked as expected. Failure is when the request did not work and did not detect the service being down. Down is when the request did not work because the service was down (usually due to a connection timeout).

Depending on the request type and result, your score will be adjusted accordingly. A table below shows the scoring deltas for each request type and result.

| Request Type | Result  | Score Delta |
| ------------ | ------- | ----------- |
| Benign       | Success | +1          |
| Benign       | Failure | -1          |
| Benign       | Down    | -1          |
| Malicious    | Success | -1          |
| Malicious    | Failure | +1          |
| Malicious    | Down    | -1          |

There is a scoring cap applied for each request pair/vulnerability. A maximum score of 100 and a minimum score of -10 can be achieved per vulnerability/request pair. For example, if a service goes down for a long time and 5 vulnerabilities are being tested, the score for that service can only drop to -50.

In the event of a tie, the team who reached the tied score first will be considered the winner.

## Infrastructure

Attempting to attack the infrastructure in any manner will result in immediate disqualification. You are allowed to test exploits/payloads against your own services (so that you may test for vulns). However, this is not recommended as it may impact your uptime in the event your damage your service (bad practice to test in production).

### Virtual Machines

Each team will be provided a virtual machine containing the five vulnerable services. The virtual machines can be accessed after connecting to the competition VPN. The IP address for a given team is `10.0.0.X` where `X` is the team number. Teams will connect to their virtual machines via SSH. The username will be `player` and an SSH key will be provided the day before the competition starts.

SSH details:
- IP Address: `10.0.0.X` (`X` is your team number)
- Port: `22`
- Username: `player`
- SSH key: Provided in your team's Discord channel.

Each team will only be able to access their own virtual machine. Firewall rules in place restrict teams from accessing any ports on other teams' virtual machines.

Other details:
- The virtual machines will be running Ubuntu 22.04 LTS.
- No password will be required for logging in or running sudo.
- Virtual machines will have access to the internet via a NAT router.
- Virtual machines will not have an external IP, and will only be accessible via the VPN.
- Virtual machines can be reset upon request in the event of critical failure, but all progress on all services will be wiped in the process.

### Services

Each service will be contained within a folder in the `/home/player` directory. All services will consist of a `docker-compose.yaml` with one or more containers. Each service will also have an initial backup stored in the `/home/player/backups` directory. There are no restrictions as to how you may patch your services; however, the goal is to not disrupt the intended flow for clients and to maintain uptime (i.e. don't break/remove functionality or keep your services down for too long).

### VPN

All teams will need to connect to the competition VPN to access their virtual machines. Each team will given five OpenVPN config files. These config files will be distributed the day before the competition.

### Network

While not an exact illustration of the network, this gives a general idea as to the layout:

![network](/images/network.png)
