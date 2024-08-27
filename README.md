# ALR-F800_RCE
## Information
**Affected product:** ALR-F800 </br>
**Vendor of the product:** Alien Technology</br>
**Product page:** https://www.alientechnology.com/products/readers/alr-f800/</br>
**Affected system version:** 19.10.24.00 and lower</br>
**Firmware download:** https://www.alientechnology.com/download/alr-f800-software/?wpdmdl=7609&ind=MTU3NTQ4MjY2NndwZG1fYWxpZW4tZmlybXdhcmVfMTkuMTAuMjQuMDBfZjgwMC5hZWQ</br>
**Shodan keyword:** http.title:"ALR-F800"</br>
**Reported by:** H0e4a0r1t</br>
## Description
ALR-F800 is a high-performance RFID reader and features Gatescape web interface.</br>
By creating a malicious ruby script under the user application function and registering the service, the malicious ruby code will be run when the corresponding service is run, resulting in an arbitrary command execution vulnerability

## Command injection vulnerability
The following is the entire process of vulnerability discovery:</br>
**URL：** http://121.152.181.70:2051/</br>
First, login to the management backend using the default password: alien/password</br>

![image](https://github.com/user-attachments/assets/ea4b205f-3d6f-45ef-8a6d-4df78949a0d9)

Afterwards, add ruby scripts containing malicious code through Administration>User Apps</br>

![image](https://github.com/user-attachments/assets/6acd0e3e-47f2-459a-9e7e-a06aee25624f)

### ruby code
```
file_path = '/var/www/1.php'
File.open(file_path, 'w') do |file|
  file.puts '<?php phpinfo();?>'
end
puts "File created and written successfully at: #{file_path}"
```

<img width="770" alt="image" src="https://github.com/user-attachments/assets/5db00ba9-4ed3-4953-b15a-4991b1177730">

upload our ruby script, like this:</br>

![image](https://github.com/user-attachments/assets/9b0e269b-5cbb-4faa-9a9e-246b8d9508fa)
 
Then right-click on the test.rb file and select Register App to register the service</br>
 
![image](https://github.com/user-attachments/assets/ff94f740-f3ba-43fe-8541-b91a567602fe)

![image](https://github.com/user-attachments/assets/0accacf0-8dae-41fa-acac-9c06b0d478b5)

Then visit Services</br>

![image](https://github.com/user-attachments/assets/b76c7354-f41e-4564-9844-2f536e5ecd52)

At this time, a 0.php will be generated in the /var/www directory, and the content is phpinfo</br>

![image](https://github.com/user-attachments/assets/ee700cc4-e9a2-41ec-b6ba-1c1b5f5e78d3)

