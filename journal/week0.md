# Week 0 — Billing and Architecture

# Week-0: Billing and Architecture
## Architecture
This section represents the requested architectural diagrams for Cruddur

I made both conceptual and Logical diagrams using Lucid charts.
I was however unable to make Adrian Cantril's Cat CI/CD Pipeline due to Lucid account restrictions. 

Lucid indicated I had run out of free icons. The lucid restriction is as seen here

 ![lucid error](images/Lucid%20error.png)

This is my submission for [Conceptual diagram](https://lucid.app/lucidchart/c50b264f-e17b-495d-9a39-5c23ddbec7a0/edit?viewport_loc=-2394%2C-322%2C2606%2C1216%2C0_0&invitationId=inv_09077203-7296-45c4-a02d-c4c66242c2c7) using Lucid chart 

![here](images/conceptual%20diagram.png)

This is my subsmission for [Logical diagram](https://lucid.app/lucidchart/c50b264f-e17b-495d-9a39-5c23ddbec7a0/edit?viewport_loc=-594%2C-151%2C2222%2C1037%2CLvNx.WRbXoQp&invitationId=inv_09077203-7296-45c4-a02d-c4c66242c2c7) using Lucid chart  

![here](images/Logical%20Diagram.png)

## Billing
For the Billing related tasks, I watched all available videos and followed [Chirag's video](https://www.youtube.com/watch?v=OVw3RrlP-sI&list=PLBfufR7vyJJ7k25byhRXJldB5AiwgNnWv&index=13) to complete this task.

I however already used this as guidance to create my budget and billing alarms before watching Andrew's video using CLI. I only created one budget in order not to exceed the free tier limit and rack up any bills.

Please find proof of my budget and billing alarms below:

Budget:
![Budget](images/Monthly%20budget.png)


Billing Alarms:
![Billing Alarms](images/Billing%20alarm.png)

## AWS Account and CLI Installation

### AWS Account (IAM User)
I first set MFA on my root account using Google Authenticator then created an IAM user(without enabling MFA) which I added to the Admin group. 

![here](images/IAM%20User.png)

All implementations henceforth were done via the created user account.
I also generated access keys via `Security Credentials` section of the user account and downloaded CSV for reference.

### AWS CLI Installation
After adding the Gitpod extension on my Chrome browser successfully, I lauched a workspace and configured AWS CLI with the string of commands below
These commands were gotten from the [AWS CLI Linux installation guide](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html):

```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

I installed on both Gitpod and local terminal.

While installing on my terminal I encountered and issue unzipping the folder because **unzip** was not installed.

![](images/unzip%20error.png)

I resolved this using the code below:

```
sudo apt install unzip
```
Here is a proof of my working AWS CLI installation:

![AWS CLI installation](images/awsidentity.png)


