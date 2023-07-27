# JWT 

<br/>
**HEADER + PAYLOAD + SIGNATURE = JWT TOKEN**<br/>

HEADER =  alg , kid <br/>
PAYDOAD  =  user info <br/>
SIGNATURE = secret key <br/>

# JWT authentication bypass via jwk header injection
**JWK INJECTION attack**<br/>

**Jo kid id hoti hai vo public or private key ka identifire hota hai. example ek shop per watier or owner hai to watier ki kid id hogi watei_123 or owner ki kid id hogi admin-123 ya kuch bhi ho sakta hai ye bus example hai to hota ye hai ki jab koi attacker JWK injection attack performe krta hai to vo esi kid ka use krta hai jo server ke db main ho or jaise attacker ne kid ko change kiya kisi public key se or uske corsponding paylod main change krke request send ki server check krega ki ye kid uske db main hai agar usko vo public key apne db main milti hai to vo us key se signature ko varify karega then usko lagega ki jwt token ke sath ko ched-chad nhi hui hai agar kid id nhi milti to jwt token invalid mana jayega**<br/><br/>

![image](https://github.com/Jeetu-study/JWT/assets/132050251/b5fff772-1244-4c68-a97e-bb6638ca02a1)<br/>

![image](https://github.com/Jeetu-study/JWT/assets/132050251/72c50873-d03a-4867-a84f-d13735885175)<br/>

![image](https://github.com/Jeetu-study/JWT/assets/132050251/dbc8ac10-2e66-482c-b389-24b225632f12)<br/>

![kwk](https://github.com/Jeetu-study/JWT/assets/132050251/0edb0346-299f-4c26-a2f9-91b57b28e402)<br/>

![image](https://github.com/Jeetu-study/JWT/assets/132050251/22b4d0bf-2d04-4972-833e-6b576a3a0720)<br/><br/>

**Mitigation**<br/>
**Transmission Security:** Public keys ko share karte waqt secure channels ka istemal karein, jaise ki SSL/TLS encryption ya secure file transfer protocols. Public key ki transmission ke dauran man-in-the-middle attacks se bachne ke liye dhyaan rakhein<br/><br/>

**Access Control:** Public keys ki access ko strictly control karein. Sirf authorized users ko access di jaye jo unhe verify karne aur use karne ke liye authorized hain.<br/><br/>


# JWT authentication bypass via jku header injection <br/>

***Jaise jwk ke under kid ka system tha ki kid asign krta tha attacker and phir server check krta tha ki uske db main koi public key hai kyuki kid ek identifire hota hai public or private key ka to jab server ko apne db main vo key mil jati thi to key valid hoti thi. but iske under kya hai JKU  ( JSON Web Key Set URL) iski full form hai phele server apne db main check krta tha but yaha per server apne db main key check krega phgir us key ko bhejega taki signature varify ho to yaha per ek url use hota hai jaha per kid identifire ka key us url per hota hai jisse dhundne main time nhi lagta or dusra key ko baar baar bhejna nhi padta ek jagha key hoti hai wahi se sab varify hota hai ab kya ho agar attacker khud ka url per apna key rakh de or usse kid ko varify krwa le to vo auth bypass kr dega***<br/>


***portswigger ki lab ke under hum khud ka server per kid ka public key rakhenge then waha se varify krwayenge***<br/>

![image](https://github.com/Jeetu-study/JWT/assets/132050251/24a6adcd-1d6b-4d94-9042-45e600eed212)<br/>

![jku](https://github.com/Jeetu-study/JWT/assets/132050251/0d0b63db-4dd7-4543-9d71-4877ee0ebd04)<br/>

![image](https://github.com/Jeetu-study/JWT/assets/132050251/337e06b0-371b-4c2b-aaf2-4fe6a5788759)<br/>

![image](https://github.com/Jeetu-study/JWT/assets/132050251/c6511e1e-1119-4a8b-af40-842c7a4d37de)<br/>

***jab kid or jku add kr diya tab uske baad sign krna hota hai means varify then send to request***<br/><br/>


# JWT authentication bypass via kid header path traversal<br/>







