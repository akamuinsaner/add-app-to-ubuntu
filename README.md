## add application to ubuntu desktop    
### example postman   

` touch /usr/share/applications/postman.desktop `  

` sudo gedit /usr/share/applications/postman.desktop `

```
#!/usr/bin/env xdg-open
[Desktop Entry]
Name=Postman
Comment=Postman
Exec=/usr/share/Postman/app/Postman
Icon=/usr/share/Postman/postman.png
Terminal=false
Type=Application
Categories=Application;Development;
StartupNotify=true
```

## .gitignore not available   

`git rm -r --cached .`


