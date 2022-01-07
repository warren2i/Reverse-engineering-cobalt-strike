# Reverse-engineering-cobalt-strike


The purpose of this repo is to document my findings.

The goal is to reverse engineer the cobalt staging powershell shellcode to become resistant to detection. Currently we have a 100% fud wallaround.

Using Rasta-mouses tool Threat-Checker to itterate line by line to find what part of the script is flagged by windows defender av, allowing us to modify the code to become av resistant. 

Cobalt strike uses some basic crypt to encode the stager shellcode. 

First the shellcode is stored in base64, this is converted from base64 to hex
This converted hex shell is then itterated over in a for loop using -bitwise xor to shift the bits, very basic crypto...


	If ([IntPtr]::size -eq 8) {
		[Byte[]]$var_code = [System.Convert]::FromBase64String($nuttystring)

	for ($x = 0; $x -lt $var_code.Count; $x++) {
		$var_code[$x] = $var_code[$x] -bxor 35
	}
  
  
  Funny thing is.... The operator bxor is actually the only component flagged by windows defender... LOL....
  
  ![alt text](https://github.com/warren2i/Reverse-engineering-cobalt-strike/blob/main/threatcheck.png)
