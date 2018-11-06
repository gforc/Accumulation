
# 查看 证书有效期
- .p12证书
openssl pkcs12 -in certificate.p12 -out certificate.pem -nodes 
- .pem证书
cat certificate.pem | openssl x509 -noout -enddate 

