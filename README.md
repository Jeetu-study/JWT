# JWT JWK JKU 

<br/>
**HEADER + PAYLOAD + SIGNATURE = JWT TOKEN**<br/>

HEADER =  alg , kid <br/>
PAYDOAD  =  user info <br/>
SIGNATURE = secret key <br/>

# 1 JWT authentication bypass via jwk header injection
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


# 2 JWT authentication bypass via jku header injection <br/>

***Jaise jwk ke under kid ka system tha ki kid asign krta tha attacker and phir server check krta tha ki uske db main koi public key hai kyuki kid ek identifire hota hai public or private key ka to jab server ko apne db main vo key mil jati thi to key valid hoti thi. but iske under kya hai JKU  ( JSON Web Key Set URL) iski full form hai phele server apne db main check krta tha but yaha per server apne db main key check krega phgir us key ko bhejega taki signature varify ho to yaha per ek url use hota hai jaha per kid identifire ka key us url per hota hai jisse dhundne main time nhi lagta or dusra key ko baar baar bhejna nhi padta ek jagha key hoti hai wahi se sab varify hota hai ab kya ho agar attacker khud ka url per apna key rakh de or usse kid ko varify krwa le to vo auth bypass kr dega***<br/>


***portswigger ki lab ke under hum khud ka server per kid ka public key rakhenge then waha se varify krwayenge***<br/>

![image](https://github.com/Jeetu-study/JWT/assets/132050251/24a6adcd-1d6b-4d94-9042-45e600eed212)<br/>

![jku](https://github.com/Jeetu-study/JWT/assets/132050251/0d0b63db-4dd7-4543-9d71-4877ee0ebd04)<br/>

![image](https://github.com/Jeetu-study/JWT/assets/132050251/337e06b0-371b-4c2b-aaf2-4fe6a5788759)<br/>

![image](https://github.com/Jeetu-study/JWT/assets/132050251/c6511e1e-1119-4a8b-af40-842c7a4d37de)<br/>

***jab kid or jku add kr diya tab uske baad sign krna hota hai means varify then send to request***<br/><br/>


# 3 JWT authentication bypass via kid header path traversal<br/>

***server ye check nhi kr raha tha ki jwt token sign hai ya nhi to ram ne AA=== null byte se jwt token ko sign kiya .. ab token server ke hisab se sign hai but vo fake taraha se sign kiya hua hai... kyuki server bus kid ko dekh raha hai to vo .././../dev/null per jata hai jaha per usko key nhi milti then vo payload ke under admin dekh kr access de deta hai .... kya ye bilul sahi hai ??*** <br/><br/>


***chatgpt Haan, aap bilkul sahi samajh rahe hain! Aapne sahi tareeke se samjha hai ki server kid ID ko check kar raha hai aur ".././../dev/null" kid ID ke liye valid key nahi milta, isliye woh usko asli key ke roop mein accept nahi karta. Lekin, Ram ne cleverly yeh dekha ki server valid keys ko accept nahi karta hai, isliye usne "AA==" (Base64 encoded null byte) ka use karke ek jhootha signature banaya.
Jab server ne token ko dekha, toh use laga ki kid ID toh valid hai, kyunki valid keys se sign hone ki expectation thi. Par Ram ne khud fake signature banayi thi, jise server ne validate nahi kiya. Isse server ne payload ko dekha aur wahan "admin" user ko dekha. Server ne ismein koi validation nahi kiya ki payload ka "admin" claim asal mein valid hai ya nahi. Is wajah se server ne Ram ko admin panel tak access de diya.
Yeh exploit tabhi kaam karta hai jab server kid ID ko dekhkar keval asal keys ke liye check karta hai. Server ko sahi tarah se token ko validate karne ki zaroorat hoti hai, jisme kid ID, signature, aur payload ko sahi tarah se verify karna hota hai. Agar server yeh validation nahi karta, toh aise chhal-kapat ka khatra hota hai jaise Ram ne kiya.*** <br/><br/>

![image](https://github.com/Jeetu-study/JWT/assets/132050251/9d3961bf-3461-4fd7-9d49-ac2485e91aee)<br/>

![path](https://github.com/Jeetu-study/JWT/assets/132050251/c0eb4d35-73dc-46a3-b764-e4646d91f6ed)<br/>

***Humne jo symetric key use ki vo as a sign use kr rahe hai ki hum jo kid per ../../../dev/null use kr rahe usko sign bhi krna hoga but humare pass valid key nhi hai to humne AA== jo base64 main hai usse sign kr diya or server ko request send krdi.. abhi yaha per server kid to dekh raha tha but ye nhi check kr raha tha ki jwt ko sign krne wali key kya hai kyuki attacker ne null byte se token ko sign kr diya or vo bypass ho gaya*** <br/><br/>


# 4 JWT authentication bypass via algorithm confusion 

***application ke is path per /jwks.json application ka private key hai jisko application token ko varify krne ke liye use kr raha hai. ab is path per jake object ko copy kiya and jwt editor ke under jake new rsa key main paste kr diya then ok or phir right click krke copy as pem kra and waha per public key copy ho jayegi then usko base46 main encode kiya the khudki ek new symtric key genrate ki and ( K ) peram ke under base64 wali value ko paste kr diya coz K peram hota hai vo sign ko varify krta hai ab humne k peram ko allredy application ki public key se replce kr diya and request main jake humne HS256 kr diya coz yaha per application only sign dekh rahi hai alg check nhi kr rahi***  <br/><br/>


***Chatgpt example Jab aapne server se public key ko /jwks.json path se uthaya, to aapke paas server ka original public key tha. Us public key ko aapne PEM format mein convert kiya.
Fir aapne JWT Editor Keys tab mein jakar ek new symmetric key generate kiya, jiska format JWK tha. Lekin server JWT token ko verify karne ke liye RSA algorithm ka use kar raha tha, jo public-private key pair ke liye hota hai. Isliye, aapne apne new symmetric key ki k property mein server ke original public key ko Base64-encoded PEM format mein daal diya.
Aapne JWT token mein alg parameter ko bhi change kar diya HS256 mein, jo symmetric key ke liye use hota hai. Jab aapne token ko sign kiya, to aapne apne new symmetric key ko use kiya, jo actually server ka public key tha.
Server ko bas token ka signature dekhna tha aur kisi bhi algorithm ki verification nahi thi, isliye usne signature ko verify kiya aur token ko valid maan liya. Isse aapne JWT authentication bypass kar liya.
Aapne sahi samjha hai ki server ne alg parameter ko nahi dekha aur bas signature verify kar liya, jisse aapne JWT authentication bypass kiya.*** <br/><br/>

![image](https://github.com/Jeetu-study/JWT/assets/132050251/79bd8aa4-edba-42db-8bf6-45f9c39aefb0)<br/>

![image](https://github.com/Jeetu-study/JWT/assets/132050251/06b44810-08b2-4dd7-8c38-0964da1e4e51)<br/>

![image](https://github.com/Jeetu-study/JWT/assets/132050251/06350c9e-d2be-4766-8e17-40797164cdd5)<br/>

![image](https://github.com/Jeetu-study/JWT/assets/132050251/a7509036-80a4-48ba-a97b-188bdd6d41ac)<br/>


# 4 JWT authentication bypass via algorithm confusion with no exposed key<br/>

***Is lab ke under bus ek cheez hai jaise last wali ke under application ki public key expose ho rahi thi but is lab ke under humne brute-force kiya hai key ko with the help of python tool . portswigger walo ka docker contanir bhi hai jiski command docker run --rm -it portswigger/sig2n <token1> <token2> .. ab iske under phele login kiya or jwt token paste kiya token1 per or dubara se logout krke login kiya and dusra jwt token ko paste kiya token2 per or enter mara ab script ye kregi ki dono token ko match kregi or token ko tempered kregi agar script ko lagega ki token tempred ho gaya to vo result show kregi kuch or humko sare jwt token laga laga kr check krna hoga ki konsa kaam kr raha hai then jonsa token kaam kr raha hoga usko rakhenge or usse related ek key bhi ayi hogi usse hum token ke under user se admin krke varify karenge***<br/>

![image](https://github.com/Jeetu-study/JWT/assets/132050251/0993f3b4-d067-4bbc-83d8-4b74a970c859)<br/>

***is image main tempred token ye hai Tampered JWT: = eyJraWQiOiJhY2U0NmEyNC1hZDRmLTRiODgtYTgzMS00MzBlOTIwODI2NjkiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiAicG9ydHN3aWdnZXIiLCAic3ViIjogIndpZW5lciIsICJleHAiOiAxNjkxMjI0OTAyfQ.phuNgs38X_uqUrIaXMyn0ybs2EAY4WBdivZdGjMXqVQ  or usse related key ye = Base64 encoded x509 key: LS0tLS1CRUdJTiBQVUJMSUMgS0VZLS0tLS0KTUlJQklqQU5CZ2txaGtpRzl3MEJBUUVGQUFPQ0FROEFNSUlCQ2dLQ0FRRUFweGF2N2Z6M2YrRUZNQm80QVg5YQpYQU1xV2FRWExPYVdxUk8rajIwWWpmSjhMMzdSM255TUlGNGdrdUptN3RteXZPK0wzUVV0OGtwaWcvSzJObGhECnNsU2d6UU9hOGJwbVBHU2pISlBrRFFpWVVDd2V1Rkp4em5adXhtbzA4YURXNmZ4UEIraEd1SU5pd3JhcTZvNW8KcnRCNW9mdHZyQVlmRTJnczZKQ2Vkakh1cThXSVlwbnNoUEwvY09Rc2JEVklYZFhnWnl1UG8ycVlyYjFxZDNwQQpJdGFyY01TWDRPdTZDbCtjVGtzVC8rZ0FNUEV5bFZ4VWVEb25Rc2VHV2J2QzRXYWU1ZFBjSGJIZHFTbVpPOXoyCmdVakMwNFprYVgvNUxUVDVSaFkyRFI0ZEVtOEtrM0dlcjJvQWxxU1hPYlQ4cjhGU3ZhM0syeVVnUzBhaVNjNWMKbVFJREFRQUIKLS0tLS1FTkQgUFVCTElDIEtFWS0tLS0tCg==  ab isi key se token ko sign krna hoga***<br/>

![image](https://github.com/Jeetu-study/JWT/assets/132050251/11217a28-f7e7-4c6d-9fc2-6a1b59e88cd4)<br/>

***ab paylod section ke under modification krenge winner to admin then token ko apni symetric key se sign kr denge***<br/>

![image](https://github.com/Jeetu-study/JWT/assets/132050251/d6556dbf-21d5-4d9e-89e5-9b0afa2c5ba2)<br/>

***and lab solved ho jayegi***














