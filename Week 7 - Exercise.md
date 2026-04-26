<h1>Question 1: Analyse packet1.pcap and find the flag.</h1>
____________________________

<img width="945" height="474" alt="image" src="https://github.com/user-attachments/assets/618ea30f-eb5d-4e21-9148-ecb12ec932ea" />

Scroll down and we could see there is one packet that is not same as others which is packet number 37. The pattern is diferent

<img width="945" height="888" alt="image" src="https://github.com/user-attachments/assets/f60d4e11-c937-4316-a577-25967071d417" />

Open the packet and we can see a weird string and we can try to use cyberchef

<img width="945" height="622" alt="image" src="https://github.com/user-attachments/assets/7a4d49e5-fabb-49db-a3c0-99e9805cf900" />

Out of all the base number, only base64 gave us the most logical decode. So i just assume that was the answer

The flag is SUCTF2023{ai_is_cool}
