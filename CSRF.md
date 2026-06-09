It is a type of security vulnerability where an attacker tricks an innocent user’s web browser into executing an unwanted action on a trusted website where the user is currently logged in.
***How it's work***
1. You login: You login into your bank account example `yourbank.com` -> Your browser saved your login session cookie
2. You visit a bad site: Without logging out your bank site, you open a new tab and visit a malicious website
3. The trap is sprung: This website contains piece of code request to your bank. For example: yourbank.com/transfer?amount=1000&to=attacker_account
4. The "browser" help the attacker: Your browser sees a request going to `yourbank.com`. Because you are still logged in, the browser automatically attaches your banking session cookie to the request.
5. The Bank is fooled: `yourbank.com` receives the request, sees your valid login cookie, assumes you authorized the transfer, and sends $1,000 to the attacker.