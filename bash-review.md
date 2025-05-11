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

    ```bash
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

    ```bash
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

    ```bash
    if [ -d /etc/pacman.d ] -->  checks if Linux distro is Arch based 

    if [ -d /etc/apt ] --> checks if Linux distro is Debian based

    release_file=/etc/os-release
    if grep -q "Arch" $release_file
    if grep -q "Ubuntu" $release_file
    ```

* For loops 

    ```bash
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
    - 1 is default and automatically implied even if we don't have it specified 
    - > overwrite, >> will append in the file 
    - standard output, standard error and standard input: using read to read from user

* Functions 
    
    ```bash
        check_exit_status() {
        if [ $? -ne 0 ]
        then
            echo "An error occured, please check the $errorlog file."
        fi
        }

        # function call 
        check_exit_status
    ```

* Case 

    ```bash
        read distro;
        Same command, but ends with a semicolon.
        The semicolon doesn’t change how the command runs — it just means:
        “End of this command; next command can come after this on the same line.”

        #!/bin/bash

        finished=0

        while [ $finished -ne 1 ]
        do
            echo "What is your favorite Linux distribution?"

            echo "1 - Arch"
            echo "2 - CentOS"
            echo "3 - Debian"
            echo "4 - Mint"
            echo "5 - Ubuntu"
            echo "6 - Something else.."
            echo "7 - Exit the script."

            read distro;

            case $distro in 
                1) echo "Arch is a rolling relase.";;
                2) echo "CentOS is popular on servers.";;
                3) echo "Debian is  a community distribution.";;
                4) echo "Mint is popular on desktops and laptops.";;
                5) echo "Ubuntu is popular on both servers and computers.";;
                6) echo "There are many distributions out there.";;
                7) finished=1 ;;
                *) echo "You didn't enter an appropriate choice."
            esac
        done

        echo "Thankyou for using the script."
    ```

    - The ;; ensures that no other cases are evaluated once one matches — kind of like a break in a C-style switch statement.


* Scheduling Jobs
    
    ```bash
    need at command 
    sudo apt install at 
    which at

    # scheduling military time 24 hours 
    at 09:34 -f ./scheduled-job-1.sh 

    # cancells the job
    artrm # - number from at queue 

    # crontab 
    crontab -e

    # m h  dom mon dow   command
    

    # edit someone else cron tab 
    sudo crontab -u username <eg. bob> -e
    ```