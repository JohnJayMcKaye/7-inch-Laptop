# 7-inch-Laptop
7 Inch Laptop, LONGEVINCE 7 Zoll Touchscreen Mini Laptop, Celeron N4000, 12GB RAM - Linux setup

#7Zoll #Linux #Notebook #Laptop #Cyberdeck #Chinashit

Diese kleinen Laptops werden sehr günstig mit Windows 11 verkauft und sind hevorragend für Linux geeignet. 
Ein paar Probleme sind allerdings zu lösen: 


- # Maussteuerung aktivieren
- Die Maustasten sind Numpad_5 und Menu was seltsam ist. Um sie zu nutzen muss die Maussteuerung aktiviert sein `gsettings set org.gnome.desktop.a11y.keyboard mousekeys-enable true` bzw. über die Gnome einstellungen.
- 
- Als script
  ```
  #!/bin/bash
  gsettings set org.gnome.desktop.a11y.keyboard mousekeys-enable false
  gsettings set org.gnome.desktop.a11y.keyboard mousekeys-enable true
  ```
- # Display Drehen
- `fbcon=rotate:1 video=DSI-1:panel_orientation=right_side_up`
  in Kernel CMDLINE eintragen z.B. mit `rpm-ostree kargs --editor`  
- # Fehlende AltGg Taste kompensieren
- NeoQUERZ-Tasataturlayout aktivieren mit: `gsettings set org.gnome.desktop.input-sources show-all-sources true`
- Ich habe dir eine passende Tastaturbeschriftung mit Inkscape erstellt, diese musst du nur auf selbstklebender Folie ausdrucken und mir Klebefolie oder Klebeband laminieren, anschließend ausschneiden und aufkleben.
  
- # Touchscreen nach Standby wieder aktivieren
- Script anlegen
  ```
  #!/bin/bash
  
  sudo modprobe -r goodix_ts
  sudo modprobe goodix_ts
  
  ```
  und mit `sudo chmod +x touchscreen.sh` ausführbar  machen  
- Dann Systemd-unit `sudo nano /etc/systemd/system/touchscreen.service`
   anlegen die nach dem Standby script ausführt  
  `
  [Unit]
  Description=Run script after waking up from suspend
  After=suspend.target
  
  [Service]
  Type=oneshot
  ExecStart=/path/to/script/touchscreen.sh
  
  [Install]
  WantedBy=suspend.target
  
  
  
  
  `   
  und mit `sudo systemctl enable touchscreen.service` aktivieren  
	- soll das script auch manuell oder per keyboard shortcut ausgeführt werden ggf. die `sudo visudo` anpassen und 
	  ```
	  #Touchscreen-dirtyfix
	  USERNAMEHERE ALL=(ALL) NOPASSWD: /path/to/script/touchscreen.sh
	  
	  ```
	  hinzufügen  
	-
- # Touchkeyboard deaktivieren
	-
	- wenn das Onscreen Keyboard aufploppt und das nerft dann kann das hiermit gefixt werden. 
	  ```
	  Touchkeyboard deaktivieren
	  
	  gsettings set org.gnome.desktop.a11y.applications screen-keyboard-enabled false
	  
	  ~/.config/autostart
	  
	  [Desktop Entry]
	  Name=BlockOSK
	  Icon=
	  Type=Application
	  Exec=gsettings set org.gnome.desktop.a11y.applications screen-keyboard-enabled false
	  Terminal=false
	  
	  oder 
	  
	  Systemd service
	  
	  nano /etc/systemd/user/blockosk.service
	  
	  [Unit]
	  Description=BlockOSK
	  [Service]
	  Type=oneshot
	  ExecStart=gsettings set org.gnome.desktop.a11y.applications screen-keyboard-enabled false
	  [Install]
	  WantedBy=graphical.target
	  
	  sudo systemctl enable blockosk.service
	  sudo systemctl start blockosk.service
	  
	  (chmod +x /full/path/to/hide-script.sh)
	  
	  ```
# Fatzit
Alles in allem ein genial kleines Gerät was mit seinen 4-5W stromverbrauch recht lange, etwa 4-5h durch hällt. Die entscheidung anstelle der Maustasten, Keyboard kommands zu verwenden ist dämlich und könnte irgendwann Probleme machen. Mit Gnome 47 startete z.B. die grafische Oberfläche nicht auf grund eines Bugs mit der aktivierten Tastaturmaus. 
Aber dennoch ein nettes kleines Cyberdeck. 
