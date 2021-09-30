# Tema 2

## Informații temă
Punctaj: 20% din total pentru laborator.
Deadline: **27 martie 2020**, se va lucra individual.

**UPDATE**: Predarea soluției se va face într-un repository de github.
Pentru a va inscrie folositi acest link: [https://classroom.github.com/a/9xUyiMpD](https://classroom.github.com/a/9xUyiMpD)

În repository scrieti sub forma de text sau markdown rezultatele voastre, puteți adăuga printscreen-uri din terminal, bucăți de cod și observații sau explicații pentru soluționarea exercițiilor. 

Pentru printscreen, asigurați-vă că este vizibil usernameul cu care faceți apelulrile din terminal.

## Cerințe

1. Citiți informațiile despre [HTTP/S din capitolul2](https://github.com/senisioi/computer-networks/tree/2020/capitolul2#https). 
2. Citiți informațiile despre [UDP din capitolul2](https://github.com/senisioi/computer-networks/tree/2020/capitolul2#socket)
3. Citiți informațiile despre [TCP din capitolul2](https://github.com/senisioi/computer-networks/tree/2020/capitolul2#tcp)


Rezolvați:
- exercițiile de la sectiune [HTTP](https://github.com/senisioi/computer-networks/tree/2020/capitolul2#exercitii_http) (3%)
- exercitiile de la secțiunea [UDP](https://github.com/senisioi/computer-networks/tree/2020/capitolul2#exercitii_udp) (10%)
- exercițiile de la secțiunea [TCP](https://github.com/senisioi/computer-networks/tree/2020/capitolul2#exercitii_tcp). (7%)



# Exemplu solutie cu markdown


## Exerciții HTTP/S
1. Cloudflare are un serviciu DoH care ruleaza pe IP-ul [1.1.1.1](https://blog.cloudflare.com/announcing-1111/). Urmăriți [aici documentația](https://developers.cloudflare.com/1.1.1.1/dns-over-https/json-format/) pentru request-uri de tip GET către cloudflare-dns și scrieți o funcție care returnează adresa IP pentru un nume dat ca parametru. Indicații: setați header-ul cu {'accept': 'application/dns-json'}.
```python
import requests

def functie(nume):
    response = requests.get('https://1.1.1.1/dns-query', params = {'name':nume}, headers = {'accept': 'application/dns-json'})
    return response.json()['Answer'][0]['data']

print(functie('fmi.unibuc.ro'))
```
---

2. Executati pe containerul `rt1` scriptul 'simple_flask.py' care deserveste API HTTP pentru GET si POST. Daca accesati in browser [http://localhost:8001](http://localhost:8001) ce observati?
```
Apare Hello World!
```
![alt text](https://github.com/Marius-RO/udp-and-tcp/blob/main/IMG/ex_2_http.jpeg)
---

3. Conectați-vă la containerul `docker-compose exec rt2 bash`. Testati conexiunea catre API-ul care ruleaza pe rt1 folosind curl: `curl -X POST http://rt1:8001/post  -d '{"value": 10}' -H 'Content-Type: application/json'`. Scrieti o metoda POST care ridică la pătrat un numărul definit în `value`. Apelați-o din cod folosind python requests.

---
Am modificat in fisierul simple_flask.py functia post_method a.i. sa afiseze valoarea transmisa, la patrat atat in momentul in care serverutl rt1 primeste requestul cat si cand clientul rt2 a primit raspunsul.

```python
@app.route('/post', methods=['POST'])
def post_method():
    print("Got from user: ", request.get_json())
    val = request.get_json()['value']
    print(val * val)
    return jsonify({'got_it': 'yes'},{'value received':val * val})

```

PS cu apelurile din terminal:

Server
![alt text](https://github.com/Marius-RO/udp-and-tcp/blob/main/IMG/ex_3_rt1_http.jpeg)

Client
![alt text](https://github.com/Marius-RO/udp-and-tcp/blob/main/IMG/ex_3_rt2_http.jpeg)

---

4. Urmăriți alte exemple de request-uri pe [HTTPbin](http://httpbin.org/)
```
Am aflat de metodele:
- HTTP, GET, POST, PUT, DELETE si de [PATCH](http://httpbin.org/#/HTTP_Methods/patch_patch)
- Auth GET 
- Status Code GET, DELETE, PATCH, POST, PUT
- Cookies GET
- Redirects GET, DELETE, PATCH, POST, PUT
```

---


## Exerciții UDP
1. Executați serverul apoi clientul fie într-un container de docker fie pe calculatorul vostru personal: `python3 udp_server.py` și `python3 udp_client.py "mesaj de trimis"`.

---
Am rulat intr-un container rt1 udp_server.py , iar intr-un alt terminal tot in containerul rt1 am rulat udp_client.py. Am trimis mesajul "Primul mesaj trimis!" de la clientul udp de pe rt1 la serverul udp de pe rt1.

Printscreen cu rezultatul:

Server
![alt text](https://github.com/Marius-RO/udp-and-tcp/blob/main/IMG/ex_1_rt1_server_udp.png)

Client
![alt text](https://github.com/Marius-RO/udp-and-tcp/blob/main/IMG/ex_1_rt1_client_udp.png)
---

2. Modificați adresa de pornire a serverului din 'localhost' în IP-ul rezervat descris mai sus cu scopul de a permite serverului să comunice pe rețea cu containere din exterior. 

----
Am modificat adresa din localhost in 0.0.0.0

```python
port = 10000
adresa = '0.0.0.0'
server_address = (adresa, port)
```

---

3. Porniți un terminal în directorul capitolul2 și atașați-vă la containerul rt1: `docker-compose exec rt1 bash`. Pe rt1 folositi calea relativă montată în directorul elocal pentru a porni serverul: `python3 /elocal/src/udp_server.py`. 

---

![alt text](https://github.com/Marius-RO/udp-and-tcp/blob/main/IMG/ex_3_udp.png)

---

4. Modificați udp_client.py ca el să se conecteze la adresa serverului, nu la 'localhost'. Sfaturi: puteți înlocui localhost cu adresa IP a containerului rt1 sau chiar cu numele 'rt1'.

---
Am modificat adresa din localhost in rt1

```python
port = 10000
adresa = 'rt1' 
server_address = (adresa, port)
mesaj = sys.argv[1]
```
---

5. Porniți un al doilea terminal în directorul capitolul2 și rulați clientul în containerul rt2 pentru a trimite un mesaj serverului:  `docker-compose exec rt2 bash -c "python3 /elocal/src/udp_client.py salut"`

--- 
Am trimis mesajul "Salut din exterior de la rt2!" de la clientul udp_client.py din rt2 modificat pt a trimite mesaje spre adresa rt1, catre serverul udp_server.py de pe rt1 ce primeste mesaje de oriunde avand adresa 0.0.0.0 .

Printscreen cu rezultatul:

Server
![alt text](https://github.com/Marius-RO/udp-and-tcp/blob/main/IMG/ex_5_rt1_server_udp.png)

Client
![alt text](https://github.com/Marius-RO/udp-and-tcp/blob/main/IMG/ex_5_rt2_client_udp.png)

6. Deschideți un al treilea terminal și atașați-vă containerului rt1: `docker-compose exec rt1 bash`. Utilizați `tcpdump -nvvX -i any udp port 10000` pentru a scana mesajele UDP care circulă pe portul 10000. Apoi apelați clientul pentru a genera trafic.

---
Am trimis mesajul "Mesaj trimis de rt2 pentru a genera trafic pentru tcpdump" folosind aceleasi setari ca la punctul 5. 

Rezultat tcpdump:
![alt text](https://github.com/Marius-RO/udp-and-tcp/blob/main/IMG/ex_6_tcpdump_udp.png)
---

7. Containerul rt1 este definit în [docker-compose.yml](https://github.com/senisioi/computer-networks/blob/2020/capitolul2/docker-compose.yml) cu redirecționare pentru portul 8001. Modificați serverul și clientul în așa fel încât să îl puteți executa pe containerul rt1 și să puteți să vă conectați la el de pe calculatorul vostru sau de pe rețeaua pe care se află calculatorul vostru.

---
Am adaugat in fisierul docker-compose.yml  "8002:8002/udp"  a.i. sa pot porni serverul udp pe portul 8002.

```yml
ports:
         - "8001:8001"
         - "8002:8002/udp"
```
Rezultat:
![alt text](https://github.com/Marius-RO/udp-and-tcp/blob/main/IMG/ex_7_porturi_udp.png)

Am modificat in udp_server.py si udp_client.py portul si adresa astfel:

```python
#udp_server.py
port = 8002
adresa = '172.9.0.2' #este interfata eth0 a containerului rt1

#udp_client.py
port = 8002
adresa = '172.9.0.2'
```
Printscreen cu rezultatul:

Server (containerul rt1):
![alt text](https://github.com/Marius-RO/udp-and-tcp/blob/main/IMG/ex_7_server_udp.png)

Client (rulat direct de pe calculator fara a folosi un container):
![alt text](https://github.com/Marius-RO/udp-and-tcp/blob/main/IMG/ex_7_client_udp.png)

tcpdump
![alt text](https://github.com/Marius-RO/udp-and-tcp/blob/main/IMG/ex_7_tcpdump_udp.png)
---


## Exerciții TCP

1. Executați serverul apoi clientul fie într-un container de docker fie pe calculatorul vostru personal: `python3 tcp_server.py` și `python3 tcp_client.py "mesaj de trimis"`.
```
prinscreen sau daca a mers la UDP, aici nu mai e necesar
```
---

2. Modificați adresa de pornire a serverului din 'localhost' în IP-ul rezervat '0.0.0.0' cu scopul de a permite serverului să comunice pe rețea cu containere din exterior. Modificați tcp_client.py ca el să se conecteze la adresa serverului, nu la 'localhost'. Pentru client, puteți înlocui localhost cu adresa IP a containerului rt1 sau chiar cu numele 'rt1'.
```
daca mers la UDP, aici nu mai e necesar
```

---

3. Într-un terminal, în containerul rt1 rulați serverul: `docker-compose exec rt1 bash -c "python3 /elocal/src/tcp_server.py"`. 

```
daca mers la UDP, aici nu mai e necesar
```

---

4. Într-un alt terminal, în containerul rt2 rulați clientul: `docker-compose exec rt1 bash -c "python3 /elocal/src/tcp_client.py TCP_MESAJ"`
---
Printscreen cu rezultatul:

Server:
![alt text](https://github.com/Marius-RO/udp-and-tcp/blob/main/IMG/ex_4_server_tcp.png)

Client:
![alt text](https://github.com/Marius-RO/udp-and-tcp/blob/main/IMG/ex_4_client_tcp.png)


---

5. Mai jos sunt explicați pașii din 3-way handshake captați de tcpdump și trimiterea unui singur byte de la client la server. Salvați un exemplu de tcpdump asemănător care conține și partea de [finalizare a conexiunii TCP](http://www.tcpipguide.com/free/t_TCPConnectionTermination-2.htm). Sfat: Modificați clientul să trimită un singur byte fără să facă recv. Modificați serverul să citească doar un singur byte cu recv(1) și să nu facă send. Reporniți serverul din rt1. Deschideți un al treilea terminal, tot în capitolul2 și rulați tcpdump: `docker-compose exec rt1 bash -c "tcpdump -Snnt tcp"` pentru a porni tcpdump pe rt1. 

---

Output tcpdump:
![alt text](https://github.com/Marius-RO/udp-and-tcp/blob/main/IMG/ex_5_tcpdump_tcp.png)

Explicatii:

Am modificat serverul si clientul astfel:
```python
#tcp_server.py
#-------
data = conexiune.recv(1) # ----> citeste un singur byte
logging.info('Content primit: "%s"', data)
#conexiune.send(b"Server a primit mesajul: " + data) ---> nu se mai trimite nimic
conexiune.close()
#-------


#tcp_client.py
#-------
sock.send(mesaj[0].encode('utf-8'))# ----> trimite un singur byte
#data = sock.recv(1024) ----> nu mai citesc nimic cu recv
#logging.info('Content primit: "%s"', data)
#-------
```

TCPDUMP:
```
Partea de 3-way handshake:
--------------------------
IP 172.9.0.2.53668 > 172.9.0.1.8001: Flags [S], seq 355326867, win 64240, options [mss 1460,sackOK,
TS val 460414540 ecr 0,nop,wscale 7], length 0

 - Clientul 172.9.0.253668 trimite o cerere de sincronizare serverului  172.9.0.1.8001, utilizand
   flagul [S] ce reprezinta o cerere de sincronizare, un sequence number generat aleator ( in cazul
   acesta seq 355326867), si lenght 0 ( mesajul contine doar header, fara payload).
 - options [mss 1460,sackOK,TS val 460414540 ecr 0,nop,wscale 7] sunt optiuni specifice pentru TCP,
 iar win 64240 este Window Size inițial.



IP 172.9.0.1.8001 > 172.9.0.2.53668: Flags [S.], seq 3908688662, ack 355326868, win 65160, options
[mss 1460,sackOK,TS val 476281348 ecr 460414540,nop,wscale 7], length 0

    - Serverul raspunde clientului prin intermediul unui mesaj trimis acestuia ce are flagul [S.] 
    (cerere de sincronizare primita), ack 355326868 (seq number trimis de client anterior + 1), 
     seq 3908688662 (la randul lui serverul trimite si el un numar generat aleator, pentru finalizarea
     3-way handshake-ului), lenght 0 (payloadul este 0, mesajul are numai header)



IP 172.9.0.2.53668 > 172.9.0.1.8001: Flags [.], ack 3908688663, win 502, options [nop,nop,TS val 460414540
ecr 476281348], length 0

    - Finalizarea 3-way handshake-ului unde clientul raspunde cu un mesaj ce contine flagul [.] (confirmare ca sincronizarea
    s-a efectuat), ack 3908688663 (confirma ca a primit seq number generat de server si trimite valoarea acestuia + 1),
    lenght 0 (la fel ca mai sus). Fiind doar o confirmare acest mesaj nu are seq number generat.


Partea de trimitere a mesajului:
--------------------------------
IP 172.9.0.2.53668 > 172.9.0.1.8001: Flags [P.], seq 355326868:355326869, ack 3908688663, win 502, 
options [nop,nop,TS val 460417544 ecr 476281348], length 1

    - Clientul trimite un mesaj serverului ce contine flagul [P.] (push , deci acum trimite payload), 
    seq 355326868:355326869 ( a trimis x byte unde x = 355326869 -  355326868, in cazul de fata x fiind 1),
    ack 3908688663 ( pana ce serverul nu va trimite si el payload clientul va confirma numai cu acest numar),
    lenght 1 (payloadul este 1, iar in acest caz a fost trimis 1 singur byte de la client la server + headerul)
    
    
    
IP 172.9.0.1.8001 > 172.9.0.2.53668: Flags [.], ack 355326869, win 510, options [nop,nop,TS val 476284352
ecr 460417544], length 0

     - Serverul confirma ca a primit mesajul trimis de client printr-un mesaj cu flagul [.] (mesaj primit), 
     ack 355326869 (se continua de la seq numarul la care a ramas clientul + 1), lenght 0 ( acest mesaj este
     doar o confirmare el avand doar header). De data aceasta fiind doar o confirmare el nu are nici seq number.
     


Partea de finalizare a conexiunii:
----------------------------------
IP 172.9.0.1.8001 > 172.9.0.2.53668: Flags [F.], seq 3908688663, ack 355326869, win 510, options [nop,nop,TS
val 476284353 ecr 460417544], length 0

    - Dupa ce primeste mesajul, serverul solicita incheierea conexiunii trimitand un mesaj cu 
    flagul [F.] (cerere de finalizare), cu seq 3908688663, ack 355326869 si lenght 0. Seq number si ack number au
    acelasi rol ca mai sus.



IP 172.9.0.2.53668 > 172.9.0.1.8001: Flags [F.], seq 355326869, ack 3908688664, win 502,
options [nop,nop,TS val 460417546 ecr 476284353], length 0

    - Clientul raspunde la solicitarea serverului tot cu un mesaj pentru incheierea conexiunii
    trimitand si el un mesaj cu flagul [F.] (cerere de finalizare) cu seq 355326869, ack 3908688664 si lenght 0.
    Seq number si ack number au acelasi rol ca mai sus.



IP 172.9.0.1.8001 > 172.9.0.2.53668: Flags [.], ack 355326870, win 510, 
options [nop,nop,TS val 476284354 ecr 460417546], length 0

    - Mesajul final de confirmare de la server catre client prin care se transmite ca cererea
    anterioara ( in acest caz cererea de finalizare ) a fost indeplinita. Fiind doar o confirmare mesajul
    nu are seq number (aici oricum nu ar trebui sa aiba deoarece conexiunea s-a incheiat in acest moment, 
    deci nu ar mai fi putut primi inapoi un mesaj cu ack seq number + 1).



Partea cu flagul [R] (reset) nu a fost capturata deoarece dupa ce conexiunea s-a incheiat clientul
nu a mai incercat sa trimita un alt mesaj serverului. Totusi, daca ar fi incercat ar fi primit un mesaj 
in care se specifica resetarea conexiunii din partea serverului.
```


