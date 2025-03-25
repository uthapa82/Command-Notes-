# Bash Script Refresher 

Notes of the learning bash script refresher.

* Determine which shell your machine is using
    - echo $SHELL --> /bin/bash 

* Make the file executable 

    `$ chmod +x <Script.sh>`

* #!/bin/bash - shebang ---> tells which interpreter a script should use 

* Lower case / Upper Case variables to distinguish environment varialbles
    - `$ env ` 

* Math functions in bash

    ```bash 
    $ expr num1 math-operator num2
    $ expr num1 \* num2 
    ```

* Conditionals- if/else statements 

    ```
    if [ $var -eq $var2 ] --> bracket test expression. More info:- man test 
    then
        do something 
    else 
        do something 
    fi 

    -eq ==> equal 
    -f  ==> find
    -v  ==> verify eg.  command -v in terminal 
    ```

* Exit codes
    - helps determine if the command was successful or not 

    ```
    echo $?
    sudo apt install $package >> package_install_results.log 
    -d --> checks for the existense of the directory 
    directory=/etc

    if [ -d $directory ]
    then
        echo "The directory exists."
    else
        echo "The directory doesn't exists."
    fi 
    ```

* While loop

    ```bash
    while [ $somevar -le 10 ]
    do
        echo $somevar
        somevar=$(( $somevar +1 ))
        sleep 0.5
    done
    ```

* Example, distro update script 

    ```
    if [ -d /etc/pacman.d ] -->  checks if Linux distro is Arch based 

    if [ -d /etc/apt ] --> checks if Linux distro is Debian based

    release_file=/etc/os-release
    if grep -q "Arch" $release_file
    if grep -q "Ubuntu" $release_file
    ```

* For loops 

    ```
    for some_var in {1..10}
    do 
        echo $some_var
        sleep 1
    done

    for file in logfiles/*.log
    do
        tar -czvf $file.tar.gz $file 
    done
    ```

* FHS- The Filesystem Hierarchy Standard 
    - run scripts from /usr/local/bin/script
    - sudo chown root:root /usr/local/bin/script
    
* Data Streams
    - where normal output goes 
    - `find /etc -type f`
    - /dev/null 
    - `find /etc -type f 1> /dev/null`
    - `find /etc -type f &> /dev/null` --> send both stdr output and error 
    - `find /etc -type f 1> results-file.txt & 2> errors-file.txt`
    - 1 -> Standard output, 2-> standard error 
    - standard output, standard error, & 

