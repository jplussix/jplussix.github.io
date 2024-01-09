---
title:  "Scripting Password Cracking Tool(Python)"
author: Vince Jang
date: 2024-01-09 00:00:00 +0000
categories: [projects]
tags: [projects]
image: /assets/img/Picture7.png
render_with_liquid: false
---

## Scripting Password Cracking Tool(Python)

There are a few popular bruth-forcing tools for password cracking such as Aircrack-ng, John the Ripper, Rainbow Crack etc..  
Brute-forcing is the best password-cracking method. The success of the attack depends on various factors. However, factors that affect most are password length and combination of characters, letters and special characters.  
  
This is why when we talk about strong password, we usually suggest that users have long passwords with a combination of lower-case letters, capital letters, numbers and special characters. It doesn't make brute-forcing impposible but it does make it difficult. Therefore, it will take a longer time(maybe forever) to reach to the password by brute-forcing.  

Almost all hash-cracking algorithms use the brute force to hit and try.  

I will try to write a script for a brute-force attack with a couple of scenarios.

## Scenarios

1. We are in the Victim's Network
2. We found user informations
3. The Passwords are stored with Hash key(Hash Value are various)

## Design and Pseudocode

First of all, I have no programming background. so my code might not look pretty but it will do the job.  

I will use an algorithm to extract user name and password from the json file(User information) I will then convert all passwords in the rockyou.txt(Password lists) to hashed values. Next, I'll check for matches. if a match is found, I will store it in the result.json file along with the matching password from the rockyou file. for the execution. I will use a combination of while and for loops to ask for file paths and display the results in the console. 

### Pseudocode

Import libraries
function to compare passwords with the rocyou.txt
get user_name and password from user_info
check if user_password in rockyou
main function
load user hashes from json
read all user hashes from json
begin main loop
use while loop to users enter for the file path or to exit
print "BYE" and exit the loop (press e)
try to load user hashed from the file by the user input in binary mode
use hash library to calculate sha256 value of the file and convert the hash to hexadecimal representiation using
store the resulting SHA256 hash in a variable.
update all changes
create dictionary containing the timestamp filepath and SHA256 hash.
Append the dictionary to the data list.
ask user for password path
check passwords using compare fuction above.
print result and save results to a results.json(do not print password on console, but results file)
print error message if user wrong input.

## Script

```
import json
import hashlib
import datetime

# Function to compare passwords with the password file.
def compare_passwords(user_hashes, rockyou):
    results = []

    for user_info in user_hashes:
        user_name = user_info["user_name"]
        user_password_hash = user_info["user_password"]

        # Check if the hashed password is in passwordfile (rockyou)
        found_password = next((password for password in rockyou if hashlib.sha256(password.encode("utf-8")).hexdigest() == user_password_hash), None)

        if found_password:
            results.append({"user_name": user_name, "password_status": "Password Found", "original_password": found_password})
            print(f"{user_name} Password Found")
        else:
            results.append({"user_name": user_name, "password_status": "Password Not Found"})
            print(f"{user_name} Password Not Found")

    return results

def load_user_hashes(file_path):
    try:
        with open(file_path, "r") as user_file:
            return json.load(user_file)
    except FileNotFoundError:
        print(f"{file_path} not found. Please make sure it exists.")
        return None
    except json.JSONDecodeError as e:
        print(f"Error decoding JSON in {file_path}: {e}")
        return None

def main():
    # Declare variables
    data = []

    while True:
        # Ask the user for input
        user_input = input("Enter the full path of the file or [e] to exit: ")

        # add exit press
        if user_input.lower() == "e":
            print("Bye!")
            break
        else:
            try:
                # Load user_hashes from the user input path.
                user_hashes = load_user_hashes(user_input)
                if user_hashes is None:
                    continue
                # Try to calculate the SHA256 value of the file
                with open(user_input, "rb") as file_to_check:
                    # Try to calculate SHA hash
                    sha256Hash = hashlib.sha256(file_to_check.read()).hexdigest()

                # the time
                timeStamp = str(datetime.datetime.now())

                # Update data
                updateVar = {"time_stamp": timeStamp, "file": user_input, "hash": sha256Hash}
                data.append(updateVar)

                # Ask the user for the password file path
                rockyouPath = input("Enter your password list file path: ")
                try:
                    # Read password file.
                    with open(rockyouPath, "r", encoding="latin-1") as rockyou_file:
                        rockyou = [line.strip() for line in rockyou_file.readlines()]
                except FileNotFoundError:
                    print("Rockyou file not found. Please enter a valid path.")
                    continue
                
                except Exception as e:
                    print(f"An error occurred while reading the rockyou file: {e}")
                    continue

                # Check passwords and print results
                results = compare_passwords(user_hashes, rockyou)

                # Append results to data
                data.extend(results)

                # Save results to results.json(Default : documents.)
                result_file_path = "results.json"
                with open(result_file_path, "w") as result_file:
                    json.dump(results, result_file, indent=2)

                print(f"password saved to {result_file_path}")

            except FileNotFoundError:
                print("File not found. Please enter again.")

if __name__ == "__main__":
    main()
```

## Test

I am going to make Random password for "4 Users" and convert into sha256 Hash value.  
It looks like this
```
[
    {
     "user_name": "Alice",
     "user_password": "8fced00b6ce281456d69daef5f2b33eaf1a4a29b5923ebe5f1f2c54f5886c7a3"
    },
    {
     "user_name": "Bob",
     "user_password": "1532e76dbe9d43d0dea98c331ca5ae8a65c5e8e8b99d3e2a42ae989356f6242a"
    },
    {
     "user_name": "Bill",
     "user_password": "9e861941ad8bf5bcb649e5fde92d712528200a216018c2437371498e6ab7683d"
    },
     {
     "user_name": "Ted",
     "user_password": "632249d0508ec3c960372a4fd4cad3fae2413619027a47b6bb5fe1c24f4a6ba4"
    }
   ] 
```  
I am going to run the Script and let's see what happened.  
It Cracked!
![images](/assets/img/Picture6.png)
![images](/assets/img/Picture6.1.png)

## Test2.
Now I want to go further in Testing, What if Hashvalue is not SHA256 but some other hashes. To do this I added a `def` function for another hash type and then run the code.  
```
[
 {
 "user_name": "Eva",
 "user_password": "4a267e19c8230e98372b5a2f12d367e8f9bd60c7abbe6117d086f308a76488e7"
 },
 {
 "user_name": "Charlie",
 "user_password": "a98f7e4c3b4b74e61c4db73d87e345f3b456dc267ee5f4c2f1a8d6b8d7a5c1f2"
 },
 {
 "user_name": "Lily",
 "user_password": "c7b8e22f9d92a45e4a8d4eac0c736e2b3a01eacdc890"
 },
 {
 "user_name": "Max",
 "user_password": "b0d9c8a7e6f5d4c3b2a1c0d9e8f7a6b5"
 },
 {
 "user_name": "Sophie",
 "user_password": "e5f4d3c2b1a0e9d8c7b6a5b4c3d2e1f0"
 },
 {
 "user_name": "Oscar",
 "user_password": "4c3b2a1c0d9e8f7a6b5e4f3d2c1a0b9"
 }
]
 ```  
 It cracked! but we didn't retreive password because password is not in the rockyou file. 
 Result:
 ![images](/assets/img/Picture7.png)
 

## User Manual
It is always good to have manual when you buy a product from JB-HiFI, so why not for this cool stuff!
![images](/assets/img/pickture8.PNG)
![images](/assets/img/picture8.1.PNG)

