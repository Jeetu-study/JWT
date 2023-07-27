# JWT 

<br/>
**HEADER + PAYLOAD + SIGNATURE = JWT TOKEN**<br/>

HEADER =  alg , kid <br/>
PAYDOAD  =  user info <br/>
SIGNATURE = secret key <br/>

**JWK INJECTION attack**<br/>

**Jo kid id hoti hai vo public or private key ka identifire hota hai. example ek shop per watier or owner hai to watier ki kid id hogi watei_123 or owner ki kid id hogi admin-123 ya kuch bhi ho sakta hai ye bus example hai to hota ye hai ki jab koi attacker JWK injection attack performe krta hai to vo esi kid ka use krta hai jo server ke db main ho or jaise attacker ne kid ko change kiya kisi public key se or uske corsponding paylod main change krke request send ki server check krega ki ye kid uske db main hai agar usko vo public key apne db main milti hai to vo us key se signature ko varify karega then usko lagega ki jwt token ke sath ko ched-chad nhi hui hai agar kid id nhi milti to jwt token invalid mana jayega**<br/><br/>

![image](https://github.com/Jeetu-study/JWT/assets/132050251/b5fff772-1244-4c68-a97e-bb6638ca02a1)<br/>

![image](https://github.com/Jeetu-study/JWT/assets/132050251/72c50873-d03a-4867-a84f-d13735885175)<br/>

![image](https://github.com/Jeetu-study/JWT/assets/132050251/dbc8ac10-2e66-482c-b389-24b225632f12)<br/>

![kwk](https://github.com/Jeetu-study/JWT/assets/132050251/0edb0346-299f-4c26-a2f9-91b57b28e402)<br/>

![image](https://github.com/Jeetu-study/JWT/assets/132050251/22b4d0bf-2d04-4972-833e-6b576a3a0720)<br/><br/>

**Mitigation**<br/>
**Transmission Security:** Public keys ko share karte waqt secure channels ka istemal karein, jaise ki SSL/TLS encryption ya secure file transfer protocols. Public key ki transmission ke dauran man-in-the-middle attacks se bachne ke liye dhyaan rakhein<br/><br/>

**Access Control:** Public keys ki access ko strictly control karein. Sirf authorized users ko access di jaye jo unhe verify karne aur use karne ke liye authorized hain.<br/>





