---
layout: post
title: Enhancing Security in Active Directory; A Comprehensive Guide to Protected Users Group 
date: 2024-08-06 21:01:00
description: Three paths to attack protected user and mitigation strategies
tags: Active-Directory
categories: Active-Directory
thumbnail: assets/img/13.jpg
---


#### What is the Protected Users Group?

The Protected Users group is a security feature introduced in Windows Server 2012 R2 and later versions of Active Directory. This group is designed to enhance the security of accounts by applying strict security policies that mitigate certain types of credential theft attacks. These policies are aimed at limiting the authentication methods and protocols that members of this group can use, thereby reducing the risk of credential exposure and misuse.

##### Key Features

1.Protected Users security group memberships restrict members to only use Advanced Encryption Standards (AES) for Kerberos. Members of the Protected Users group must be able to authenticate using AES.

2.No NTLM Authentication: NTLM authentication is disabled for members of the Protected Users group. NTLM is a less secure protocol and is more susceptible to credential theft attacks like pass-the-hash.

3.Kerberos Ticket Restrictions: The lifetime of Kerberos Ticket Granting Tickets (TGTs) for Protected Users is limited to 4 hours by default, and these tickets cannot be renewed. This reduces the window during which a stolen ticket can be misused.

4.No Cached Credentials: Credentials for members of the Protected Users group are not cached on the local machine. This prevents attackers from retrieving cached credentials if they gain physical or remote access to a computer.

5.Interactive Logon Restrictions: Members cannot log on interactively to computers that do not support the security policies enforced by the Protected Users group. This ensures these high-value accounts are only used on secure and trusted systems.

#### Why Use the Protected Users Group?

The primary reason for utilizing the Protected Users group is to provide enhanced security for high-value accounts. These accounts are often targeted by attackers due to their elevated permissions and access to sensitive data.


#### Potential Attacks

Despite the enhanced security measures provided by the Protected Users group, attackers may still attempt to target these high-value accounts. Understanding these potential attacks and how to mitigate them is crucial.

let's say we have a user "admin2", who is a regular domain admin. We performed a password spray attack and gain access allowing us to dump the lsass process and obtain its hash. 

<div class="row" style="text-align: center;">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" max-width="855px" max-heigth="708px" path="assets/img/1.JPG" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

<div class="row" style="text-align: center;">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" max-width="455px" max-heigth="408px" path="assets/img/2.JPG" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

Next, we added "admin2" to the protected users group and attempt the password spray and pass-the-hash attacks. 


<div class="row" style="text-align: center;">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" max-width="455px" max-heigth="408px" path="assets/img/3.JPG" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

<br>This time, we receive an error (status account restriction) indicating that it no longer works.<br>


<div class="row" style="text-align: center;">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" max-width="755px" max-heigth="608px" path="assets/img/4.JPG" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>


<div class="row" style="text-align: center;">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" max-width="755px" max-heigth="608px" path="assets/img/5.JPG" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

<b> Finally, </b> we dump the lsass process again and find that there is no hash available


<div class="row" style="text-align: center;">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" max-width="455px" max-heigth="408px" path="assets/img/3-1.JPG" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

##### First Attack Scenario:

We can target the protected users group (admin2). If we manage to crack the obtained NTLM hash before, we can use the Rubeus to generate an AES256 hash using the clear text password.


<div class="row" style="text-align: center;">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" max-width="455px" max-heigth="408px" path="assets/img/6.JPG" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

 Using Rubeus, we can then successfully request a TGT ticket. 



<div class="row" style="text-align: center;">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" max-width="455px" max-heigth="408px" path="assets/img/7.JPG" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

##### Second Attack Scenario:

If we gain access to a machine where the admin2 user is already signed in, we can dump the TGT ticket. 


<div class="row" style="text-align: center;">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" max-width="455px" max-heigth="408px" path="assets/img/8.JPG" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

Next, we can successfully load it in memory.


<div class="row" style="text-align: center;">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" max-width="455px" max-heigth="408px" path="assets/img/9.JPG" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

##### Third Attack Scenario:
password spray attack on users in protected group, we can target a specific user by using the Netexec with --Kerberos flag, as shown below.


<div class="row" style="text-align: center;">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" max-width="555px" max-heigth="455px" path="assets/img/10.JPG" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

#### Mitigation Strategies

1. Using a complex password: A strong password typically combines uppercase and lowercase letters, numbers, and special characters, making it harder for attackeers to guess or crack. Avoid using easily guessable information like birthdays or common words. Regularly updating your passwords and using unique ones for different accounts further enhances your security.

2. Regular Monitoring and Auditing: Continuously monitor and audit the activities of members in the Protected Users group. Look for suspicious logins and unusual activities that may indicate a compromise.

3. Use Secure Systems: Ensure that members of the Protected Users group only log in to systems that are up-to-date with security patches and that support the necessary security measures.

4. Implement Additional Layers of Security: Use additional security measures such as multi-factor authentication (MFA), privileged access management (PAM) solutions, and network segmentation to further protect high-value accounts.

5. Educate and Train Users: Regularly educate and train users on security best practices and the importance of safeguarding their credentials. Awareness can significantly reduce the risk of credential theft.


#### Conclusion 

The Protected Users group in Active Directory is a powerful tool for enhancing the security of high-value accounts by enforcing stringent security policies and mitigating common credential theft attacks. By understanding what this group is, why it should be used, when to use it, and the potential attacks against it, organizations can effectively safeguard their most of the high-value accounts and ensure a higher level of security across their Active Directory environment. Regular monitoring, system updates, additional security measures, and user education are key components in maintaining the integrity of the Protected Users group and the overall security posture of the organization.

#### Additional Resources

To further understand the Protected Users group and its implementation, refer to the following resources:

[Microsoft Protected Users Security Group](https://learn.microsoft.com/en-us/windows-server/security/credentials-protection-and-management/protected-users-security-group)

[Microsoft Guidance about how to configure protected accounts](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/how-to-configure-protected-accounts)

