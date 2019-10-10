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

## Java environment variables setting    
open .bashrc   
```
export JAVA_HOME=/usr/share/java
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:JAVA_HOME/lib:JRE_HOME/lib:${CLASSPATH}
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
```

## Android Studio download   

[Android Studio](https://developer.android.google.cn/studio)




