* To include the env folder to gitignore 
    
    `$ echo 'env' > .gitignore`

* Store the installed packages in requirements.txt file 
    
    `$ pip freeze > requirements.txt`

* Install the requirements required to run the project 
    
    `$ pip install -r requirements.txt`

* ip configuration on windows device 
    
    `C:>netsh interface ipv4 set address name="Ethernet" static <ip-address> <subnet-mask> <gateway>`

* Change DNS server on windows device 
    
    `C:>netsh interface ipv4 set dns name="Ethernet" static <ip-address> `

* save the output of ipconfig in a file Windows 

    `> ipconfig /all >$env:USERPROFILE:DESKTOP\ipconfig.txt`

    