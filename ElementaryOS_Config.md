Elementary OS Configuration

Stuff to do after install

Update

sudo apt update && sudo apt upgrade

tray icons: 
```
 wget https://github.com/Lafydev/wingpanel-indicator-ayatana/raw/master/com.github.lafydev.wingpanel-indicator-ayatana_2.0ubuntu6_amd64.deb 
 wget https://github.com/mdh34/elementary-indicators/releases/download/0.1/indicator-application-patched.deb 
 sudo dpkg -i com.github.lafydev.wingpanel-indicator-ayatana_2.0ubuntu6_amd64.deb indicator-application-patched.deb 
 sudo apt-mark hold indicator-application 
 sudo reboot
```
 unity editor fix
 
 add to ~/.local/share/applications/UnityEditor.desktop
 
 #!/usr/bin/env xdg-open
[Desktop Entry]
Type=Application
Terminal=false
Icon=utilities-terminal
Exec=bash -c "/home/ruitx/Applications/UnityEditor.sh"
Name=Unity Editor - Compatibility Mode
Comment=Compatibility mode for Unity Editor in Elementary OS

add to ~/Applications/UnityEditor.sh
#!/bin/sh
# https://forum.unity.com/threads/scrolling-editor-unity-2018-2-2f1.544847/
# GTK_CSD=0 GTK3_MODULES="" /path/to/Unity/Editor/Unity./Unity -projectPath /path/to/Project/File/Root
EditorLocation=~/Unity/Hub/Editor/2020.1.10f1/Editor/Unity
ProjectLocation=$(zenity --text="Please select a folder." --file-selection --directory)
if [ -z "$ProjectLocation"]
then
  zenity --error  --width=256 --text="Please select a project."
  echo "No project selected, exiting..."
else
  echo "Loading ${ProjectLocation}"
  GTK_CSD=0 GTK3_MODULES="" ${EditorLocation}\
   -projectPath  ${ProjectLocation}
fi
echo "Done."
exit

open terminal and chmod +x ~/Applications/UnityEditor.sh


 
