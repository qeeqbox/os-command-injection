<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/os-command-injection/main/content/os-command-injection.svg"></p>

An application utilizes operating system (OS) commands to perform actions. A threat actor can exploit this feature by tricking the application into executing a malicious payload containing OS commands

Clone this current repo recursively
```sh
git clone --recursive https://github.com/qeeqbox/os-command-injection
```
Run the webapp using Python
```sh
python3 os-command-injection/vulnerable-web-app/webapp.py
```
Open the webapp in your browser 127.0.0.1:5142
<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/os-command-injection/main/content/1.png"></p>
Use the default credentials (username: admin and password: admin) to login
<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/os-command-injection/main/content/2.png"></p>
The application allows users to check network connectivity using the host's ping OS command, enter 127.0.0.1
<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/os-command-injection/main/content/3.png"></p>
The ping OS command is executed
<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/os-command-injection/main/content/4.png"></p>
A threat actor could use logical opertator (&, &&, | or ||) or commands seperators (;) to make the host run extra commands
<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/os-command-injection/main/content/5.png"></p>
The host executed the ping command and the whoami command
<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/os-command-injection/main/content/6.png"></p>

## Code
When the user enters a hostname or IP to check their network connectivity, the webapp calls the add_ping() function. This function uses the internal ping OS command, the dynamic value from the user can contain a malicious payload that also gets executed by the host
```py
@logged_in
@check_access(access="ping")
def add_ping(self, ping):
    with Popen("ping -c 1 " + ping, stdout=PIPE, stderr=STDOUT, shell=True) as process, connect(DATABASE, isolation_level=None) as connection:
        cursor = connection.cursor()
        cursor.execute("INSERT into ping(username, ping, output) values(?,?,?)", (self.session["username"], ping, process.communicate()[0].decode("utf-8")))
        return True
    return False
```
 
## Impact
Critical

## Risk
- Session Hijacking
- Credential Theft

## Redemption
- Server input validation

## ID
cb251c97-067d-4f13-8195-4f918273f41b
