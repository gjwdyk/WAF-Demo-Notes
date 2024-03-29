# Inspect UserName and Password Input Field on Chrome (including Microsoft Edge)

Obtaining the "username" and "password" information from Input Field of a browser depends on:
- [ ] The Browser itself. Some browsers provide shortcut commands, others don't. But they all share a formal syntax to obtain the "username" and "password" information from the Input Field.
- [ ] The Source Code of the page itself. Each pages structured uniquely, differently depending on the developer. Study towards the page structure must be done BEFORE the demo.



## DVWA

![DVWA Rendered](DVWA_Rendered.png)

On the rendered login screen, enter a User Name and Password information for the demo. Then right-click on the "usename" field, and choose menu "Inspect". The "Elements" window will focus on the "username" field. Study the structure of the "username" element.

![DVWA Elements UserName](DVWA_Elements_UserName.png)

Similarly, on the rendered login screen, right-click on the "password" field, and choose menu "Inspect". The "Elements" window will focus on the "password" field. Study the structure of the "password" element.

![DVWA Elements Password](DVWA_Elements_Password.png)

On "Console" tab, enter the commands :

```
document.getElementsByClassName("loginInput").username.value
document.getElementsByClassName("loginInput").password.value
```

![DVWA Console](DVWA_Console.png)

After each command, the previously entered User Name and Password information will be printed out on the console.



## Hackazon

![Hackazon Rendered](Hackazon_Rendered.png)

On the rendered login screen, enter a User Name and Password information for the demo. Then right-click on the "usename" field, and choose menu "Inspect". The "Elements" window will focus on the "username" field. Study the structure of the "username" element.

![Hackazon Elements UserName](Hackazon_Elements_UserName.png)

Similarly, on the rendered login screen, right-click on the "password" field, and choose menu "Inspect". The "Elements" window will focus on the "password" field. Study the structure of the "password" element.

![Hackazon Elements Password](Hackazon_Elements_Password.png)

On "Console" tab, enter the commands :

```
document.getElementById("username").value
document.getElementById("password").value
```

![Hackazon Console](Hackazon_Console.png)

After each command, the previously entered User Name and Password information will be printed out on the console.



<br><br><br>
```
╔═╦═════════════════╦═╗
╠═╬═════════════════╬═╣
║ ║ End of Document ║ ║
╠═╬═════════════════╬═╣
╚═╩═════════════════╩═╝
```
<br><br><br>


