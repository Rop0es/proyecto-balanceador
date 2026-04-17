# Practica: Proxies amb Nginx i Infraestructura Docker Compose

Aquest repositori conte la meva solucio per a la practica de desplegament d'una infraestructura web d'alta disponibilitat. El sistema utilitza Docker per gestionar dos nodes de servidor Apache darrere d'un proxy invers Nginx que realitza el balanceig de carrega i gestiona la memoria cau.

## Fases de desenvolupament

He seguit els passos indicats a la guia per construir la infraestructura de forma incremental:

Fase 1 i 2: He posat en marxa la base del sistema amb dos contenidors Apache. He creat una pagina web que inclou text, imatges i un video. Per verificar quin node respon cada peticio, he utilitzat els logs de Docker.

Fase 3: He configurat un volum compartit perque els dos nodes Apache serveixin exactament el mateix contingut des de la carpeta local ./html. D'aquesta manera, qualsevol canvi en els fitxers es reflecteix automaticament en ambdos servidors.

Fase 4: He afegit Nginx com a punt d'entrada principal. He configurat el fitxer de Nginx amb un bloc upstream per distribuir el transit entre els dos servidors Apache utilitzant l'algorisme Round Robin.

Fase 5: He activat la memoria cau (proxy cache) a Nginx per optimitzar el lliurament del contingut. He configurat capçaleres HTTP personalitzades per poder identificar des del navegador si la resposta ve de la cache (HIT) o del servidor d'origen (MISS).

## Problemes trobats i solucions

1. Actualitzacio de la cache: Inicialment, quan feia canvis al codi HTML, no els veia reflectits al navegador. Vaig comprendre que era perque la cache de Nginx estava funcionant i servia la versio antiga. La solucio va ser esperar el temps d'expiracio o netejar la carpeta de cache de Nginx.

2. Visualitzacio del video: El video no s'executava sol en carregar la pagina. Vaig haver de modificar l'etiqueta HTML per incloure l'atribut 'muted', ja que els navegadors actuals bloquegen l'autoreproduccio de videos amb audio per defecte.

3. Identificacio del backend: Al principi no sabia segur si el balanceig funcionava. Vaig resoldre-ho executant la comanda 'docker-compose logs -f', que em permet veure en temps real quin dels dos contenidors Apache rep cada peticio.

## Instruccions d'instal·lacio i execucio

Per fer funcionar aquest projecte a la teva maquina, segueix aquests passos:

1. Clonar el repositori:
git clone https://github.com/Rop0es/proyecto-balanceador.git
cd proyecto-balanceador

2. Aixecar la infraestructura:
docker-compose up -d

3. Accedir a la web:
Obre el navegador i entra a http://localhost

## Evidencies de funcionament

A continuacio s'han d'adjuntar les captures de pantalla que demostren el compliment dels requisits:

- Captura de la web funcionant (text, imatges i video).
- <img width="1499" height="621" alt="image" src="https://github.com/user-attachments/assets/4bb604da-775c-4dcd-b033-2955a91489c9" />

- Captura dels logs de la terminal on es veu el balanceig de carrega Round Robin entre nodes.
- <img width="961" height="431" alt="image" src="https://github.com/user-attachments/assets/414a3231-4b49-48f4-8bc7-56ca6b83b4d9" />

- Captura de les eines de desenvolupador del navegador on es veu la capçalera de cache (HIT/MISS).
- <img width="746" height="443" alt="image" src="https://github.com/user-attachments/assets/911688c3-0857-4b0b-8d63-5fe851b0e13f" />
