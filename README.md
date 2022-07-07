## Preparación para la OSCP - Metodología & Scripts

![banner_oscp](https://user-images.githubusercontent.com/87484792/177843931-081eca92-24f1-4743-a632-48ee65b2ba4a.png)

# Enumeración - Fase Inicial
## Enumeración de puertos

`sudo nmap --minrate-5000 -p- -vvv -Pn -n -oG openPorts.txt <ip>` # Encontrar puertos abiertos con nmap </br>

`sudo nmap -sSCV -p{ports} -oN servicesPorts.txt <ip>` # Verificar que servicios hay en los puertos encontrados.

`sudo nmap --script=http-enum  -p {port http/s} <ip>` # Pequeño script de nmap (".nse") para la verificación de directorios en http/s

`sudo nmap -sU -T5 --top-ports 500 <ip> ` # Encontrar puertos abiertos UDP con nmap

## Enumeración de directorios - Fuzzing 

`wfuzz --hc 404 -c -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt http://<ip>/FUZZ` # Encontrar directorios disponibles con wfuzz

`wfuzz --hc 404 -c -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -H "cookie: SSID=dasd45ads6aa5s" http://<ip>/FUZZ` # Encontrar directorios disponibles con cookies

`wfuzz --hc 404 -c -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -d "id=FUZZ&catalogue=1" <ip>` # Fuzzear data

`dirsearch --url http://ip/` # Fuzzear con dirsearch

`dirsearch --url http://ip/ -e .txt,.aspx` # Fuzzear con dirsearch con extensiones propias

`dirsearch --url http://ip/ -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt` # Fuzzear con dirsearch con diccionario propio

```
Tips:
** Enumerar directorios ocultos "http://ip/.FUZZ/"
** Enumerar directorios dentro de otros directorios "http://ip/admin/FUZZ"
** En wfuzz, puedes skyp el certificado SSL con la bandera "-k"
** Puedes usar hilos para agilizar, no aconsejable poner más de 200 "-t 200"
```

## Enumeración de subdominios - Fuzzing 

`wfuzz --hc 404 -c -w /usr/share/amass/wordlists/subdomains-top1mil-110000.txt -H "HOST: FUZZ.domain.htb" domain.htb` # Fuzzear con wfuzz subdominios

`gobuster vhost --url domain.htb --wordlist /usr/share/amass/wordlists/subdomains-top1mil-110000.txt` # Fuzzing con gobuster subdomains


