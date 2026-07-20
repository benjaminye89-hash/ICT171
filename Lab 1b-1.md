## Apache WebServer installed
<img width="1422" height="968" alt="Screenshot From 2026-07-19 22-55-21" src="https://github.com/user-attachments/assets/0a0be3a2-4971-4a56-ab5e-f85444666981" />

## Nmap installed
<img width="952" height="398" alt="image" src="https://github.com/user-attachments/assets/cf7594fa-e5dd-4686-a904-bfa6063d5c5a" />

Nmap scan before Apache is removed
<img width="952" height="398" alt="image" src="https://github.com/user-attachments/assets/53fb0b10-2e7e-4d7d-8c66-b6ecb056990e" />

Stop Apache service
<img width="973" height="106" alt="image" src="https://github.com/user-attachments/assets/e9e287a0-3955-4322-8aec-17d76ef0c0db" />

Nmap scan after Apache is removed
<img width="958" height="365" alt="image" src="https://github.com/user-attachments/assets/4322a54d-ae14-4042-9eb5-d4e6f2bff656" />

## Firewall Setup
"sudo ufw status verbose"
UFW is currently inactive. This means there are no active network traffic restrictions, and all ports are exposed to the local network.
<img width="961" height="107" alt="image" src="https://github.com/user-attachments/assets/8b0f8e53-fa23-4340-a60b-4f49c393ba44" />

"sudo ufw enable"
<img width="962" height="132" alt="image" src="https://github.com/user-attachments/assets/fbaca0cb-4c9a-4805-a010-a9992aa1b693" />

sudo ufw status verbose
UFW is now active. It has applied a default-deny policy, meaning it blocks all incoming connections. I have explicitly added 'allow' rules for ports 22 (SSH) and 80 (HTTP) so I can still access the server and web browser.
<img width="962" height="132" alt="image" src="https://github.com/user-attachments/assets/a11ed398-eff5-4db1-923b-81f7c9debacc" />

## SSH & User Management
SSH enabled and working
<img width="968" height="670" alt="image" src="https://github.com/user-attachments/assets/da8bdf83-cdb7-493b-bfa8-cd5038cd7838" />

### Successfully SSH'd into own machine
<img width="961" height="835" alt="image" src="https://github.com/user-attachments/assets/fe07c3b8-d494-4734-9f8a-0d884116ac83" />

### New user created
<img width="940" height="421" alt="image" src="https://github.com/user-attachments/assets/276dd9bb-46b4-4780-813e-c54cfee726be" />
### Verify user
<img width="970" height="298" alt="image" src="https://github.com/user-attachments/assets/6eb97d93-954b-4d1f-adff-bc4c0274df44" />
### Test login
<img width="935" height="597" alt="image" src="https://github.com/user-attachments/assets/03066501-270d-4101-8609-632e6ac816ac" />

## Compression and File transfer
### Goal: Bundle files, compress them, and move them around.
### Compression/Decompression Tested
<img width="943" height="397" alt="image" src="https://github.com/user-attachments/assets/23951d15-b68e-44d0-b46b-3491062b79c8" />
<img width="818" height="102" alt="image" src="https://github.com/user-attachments/assets/ea48ab79-d9da-4fbf-8879-9a73c00afec4" />


##SCP File Transfer
<img width="927" height="215" alt="image" src="https://github.com/user-attachments/assets/97644ef5-7f6d-4ed4-99b4-c345a595a5f1" />




