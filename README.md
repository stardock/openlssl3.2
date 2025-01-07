# query

从私钥提取公钥  
`openssl rsa -pubout -in private-key.pem -out public-key.pem`  

openssl 从cer文件中提取公钥  
`openssl x509 -in XX.cer -pubkey  -noout > XX.pem`  

Ref:  
https://blog.csdn.net/albertsh/article/details/134818171

