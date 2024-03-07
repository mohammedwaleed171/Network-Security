# Network Security

### We will be using Tryhackme's NetSec Challenge room to answer scenario based questions. This room is the culmination of the Network Security module, requiring familiarity with the use of Nmap to scan networks, Telnet to act as a client for engaging with different protocols and Hydra for performing dictionary attacks on weak login credentials.
### The purpose of this room is to have an understanding and learn about passive reconnaissance, active reconnaissance, Nmap, protocols and services, and attacking logins with Hydra.

## Q1: What is the highest port number being open less than 10,000?
  - Run a basic nmap scan on the target
    <p align="center"><img src="https://miro.medium.com/v2/resize:fit:640/format:webp/1*WGq0oKawreQ8Tzm4IaPMHQ.png" height="40%" width="40%" /><p/></p> <br/>

## Q2: There is an open port outside the common 1000 ports; it is above 10,000. What is it?
  - Run a basic nmap scan with the -p flag to specify all ports above 10000 and the --min-rate flag to specify the minimum number of concurrent requests nmap can try to maintain. This will speed up the scan massively.
    <p align="center"><img src="https://miro.medium.com/v2/resize:fit:828/format:webp/1*7MkJWA1Io57veUbc-xjncQ.png" height="40%" width="40%" /><p/></p> <br/>
## Q3: How many TCP ports are open?
  - In total 6 open ports have been found above.
## Q4: What is the flag hidden in the HTTP server header?
  - Use telnet to connect to the target on port 80, which is hosting the http service, as found in the results of the nmap scan in Q1. Construct a basic GET request to the web server and observe the flag in the response.
  <p align="center"><img src="https://miro.medium.com/v2/resize:fit:720/format:webp/1*H3rLEtEtP84ieRRLNVhpjQ.png" height="40%" width="40%" /><p/></p> <br/>

## Q5: What is the flag hidden in the SSH server header?
  - Now use telnet to connect to the target on port 22, which is hosting the ssh service, as found in the results of the nmap scan in Q1. Connecting automatically reveals the flag.
  <p align="center"><img src="https://miro.medium.com/v2/resize:fit:640/format:webp/1*U0MlOjg6LuvOMcm6qj5KqQ.png" height="40%" width="40%" /><p/></p> <br/>

## Q6: We have an FTP server listening on a nonstandard port. What is the version of the FTP server?
  - Use the -A flag to perform an aggressive scan of the target. This enables OS and version detection. Specify all the open ports with the -p flag. It is preferable to only run an aggressive scan on open ports as it is a waste of time and resources to aggressively scan closed ports, hence why it is better to first find the open ports and then only perform an aggressive scan on them.
    <p align="center"><img src="https://miro.medium.com/v2/resize:fit:828/format:webp/1*kE4emAMOEcQQ6dSN1whL2g.png" height="40%" width="40%" /><p/></p> <br/>

## Q7: We learned two usernames using social engineering: eddie and quinn. What is the flag hidden in one of these two account files and accessible via FTP?
  - Given some usernames, it may be possible to use hydra to perform a dictionary attack on the FTP server and discover their passwords.
  - Construct a hydra attack on the target by using the following flags and arguments:
      - -l to specify the login username. Do one attack for eddie and for quinn
      - -P to specify the wordlist of passwords to try. I have used rockyou-50.txt from seclists.
      - The target machine’s IP
      - The service hydra is attacking. In this case it is ftp
      - -s to specify the port the service is using. This is crucial as in this instance ftp is being hosted on a non-standard port.
<p align="center"><img src="https://miro.medium.com/v2/resize:fit:828/format:webp/1*8wjWyHAIkWeULGkEP9yGGA.png" height="40%" width="40%" /><p/></p> <br/>
<p align="center"><img src="https://miro.medium.com/v2/resize:fit:828/format:webp/1*_892nABGlJK9PqwXNnuxHg.png" height="40%" width="40%" /><p/></p> <br/>

  - Now that 2 sets of login credentials have been found, log in to the webserver’s ftp server. Remember to specify the non-standard port with the -p flag. Use ls to search for any files.
    <p align="center"><img src="https://miro.medium.com/v2/resize:fit:640/format:webp/1*Tz4UTC0Q7s0Rs2s5HdpwVw.png" height="40%" width="40%" /><p/></p> <br/>
    <p align="center"><img src="https://miro.medium.com/v2/resize:fit:828/format:webp/1*Ho_9S29YJpYh3tokL4LVbA.png" height="40%" width="40%" /><p/></p> <br/>
  - The flag was found to be in quinn’s folder. Use get to download the file to your local machine. Inspect it with cat to read its contents.
    <p align="center"><img src="https://miro.medium.com/v2/resize:fit:504/format:webp/1*BzbTAyQNy5TVlqu4vwVODQ.png" height="40%" width="40%" /><p/></p> <br/>

## Q8: Browsing to http://MACHINE_IP:8080 displays a small challenge that will give you a flag once you solve it. What is the flag?
  - Accessing the IP of the target machine on port 8080 in the web browser reveals this page. The goal is to perform a scan that doesn not get detected.
    <p align="center"><img src="https://miro.medium.com/v2/resize:fit:828/format:webp/1*r9ah1Ua9ZM-aHC9W1V-shg.png" height="40%" width="40%" /><p/></p> <br/>
  - This can be solved by performing a Null Scan using the -sN flag.
    <p align="center"><img src="https://miro.medium.com/v2/resize:fit:828/format:webp/1*_KmHfgKimOgBGPajLQiVpQ.png" height="40%" width="40%" /><p/></p> <br/>
  - The Null scan involves sending a TCP packet with no set flags. No response is received from open ports but closed ports will respond with RST, allowing identification of the open ports by deduction.
  - Performing the Null scan successfully avoids detection by the IDS, granting the final flag!
    <p align="center"><img src="https://miro.medium.com/v2/resize:fit:828/format:webp/1*P0jctXWIqHy7APIQeuemPA.png" height="40%" width="40%" /><p/></p> <br/>
