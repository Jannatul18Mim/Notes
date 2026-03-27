Click the Start Menu, type PowerShell, and open it.

Copy and paste the code below and press Enter.

For Windows 8.1, 10 and 11:

     irm https://get.activated.win | iex

<img width="1306" height="358" alt="image" src="https://github.com/user-attachments/assets/53c398c8-e659-4acb-9f69-2cf66e882331" />
     

<img width="647" height="560" alt="image" src="https://github.com/user-attachments/assets/c11988a0-79ce-4ccd-badd-c0437f1eddbc" />


If the above is blocked (by ISP/DNS), try this (needs updated Windows 10 or 11):

    iex (curl.exe -s --doh-url https://1.1.1.1/dns-query https://get.activated.win | Out-String)

In the menu that appears, type the number corresponding to one of the Green options.
