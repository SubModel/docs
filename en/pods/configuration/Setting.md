# SSH & API Key Management Documentation  

## 1. SSH Public Key Configuration  
### Overview  
Secure instance access using SSH key pair authentication.  

### Procedure  
1. Generate key pair (local terminal):  
   ```bash
   ssh-keygen -t ecdsa -b 521 -f ~/.ssh/instance_key
   ```  
2. Upload public key (`instance_key.pub`) via management console  
3. Connect to instance:  
   ```bash
   ssh -i ~/.ssh/instance_key user@instance-ip
   ```  

### Security Requirements  
- Set private key permissions to 600  
- ECDSA or EdDSA algorithms required  
- Rotate keys quarterly  

## 2. API Key Management  
### Overview  
Credentials for programmatic API access.  

### Key Handling  
1. Generate keys via console interface  
2. Include in requests:  
   ```http
   x-apikey: YOUR_API_KEY
   ```  

### Security Policy  
- Treat as sensitive credentials  
- Immediate revocation if exposed  
- 90-day rotation mandatory  

## 3. Registry Authentication  
### Supported Registries  
- Docker Hub  
- AWS ECR  
- GCR  

### Configuration  
1. Provide credentials in format:  
   ```
   Username: [registry_user]
   Password: [registry_token]
   Endpoint: [registry_url]
   ```  
2. Select credentials when creating templates  

### Troubleshooting  
- Verify credential permissions for 403 errors  
- Include protocol (https://) in server addresses  

## Best Practices  
- Unique SSH keys per environment  
- API key segregation by service tier  
- Temporary registry tokens preferred  
