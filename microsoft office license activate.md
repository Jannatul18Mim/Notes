Click the Start Menu, type PowerShell, and open it.

Copy and paste the code below and press Enter.

For Windows 8.1, 10 and 11:

     ```irm https://get.activated.win | iex```

If the above is blocked (by ISP/DNS), try this (needs updated Windows 10 or 11):

    ```iex (curl.exe -s --doh-url https://1.1.1.1/dns-query https://get.activated.win | Out-String)```

In the menu that appears, type the number corresponding to one of the Green options.
