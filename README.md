# ESERCITAZIONE DOCKER FILE
## struttura del progetto

mkdir hello-docker
cd hello-docker
👉 Crea una cartella chiamata hello-docker e ci entri. Tutti i file del progetto verranno messi qui dentro.

# passo 2
## crea il file app.js

[apri] nano app.js
Incolla questo codice:

const express = require('express');       // Importa il modulo Express
const app = express();                    // Crea un'applicazione Express
const port = 3000;                        // Imposta la porta su cui l'app ascolterà

app.get('/', (req, res) => {              // Definisce una route GET per "/"
    res.send('Hello, World!');            // Quando riceve una richiesta, risponde con 'Hello, World!'
});

app.listen(port, '0.0.0.0', () => {       // Avvia il server sull'indirizzo 0.0.0.0 (accessibile anche da fuori)
    console.log(`App listening at http://localhost:${port}`);
});

# 🔍 Cosa fa questo codice?
Crea un piccolo web server con Express

Risponde "Hello, World!" quando accedi alla homepage

Ascolta sulla porta 3000

0.0.0.0 permette di accedere anche dall'esterno (es: da un browser sul tuo PC se l'app gira su una VM)

#🔧 Se lasciassi solo localhost, la VM non permetterebbe connessioni esterne.

# passo 3
## crea il file package.jsom 

🔨 Questo file descrive il tuo progetto Node.js
Crealo con:

Incolla questo contenuto:

{
  "name": "hello-docker",               // Nome del progetto
  "version": "1.0.0",                   // Versione del progetto
  "main": "app.js",                     // File principale da eseguire
  "scripts": {
    "start": "node app.js"             // Comando "npm start" eseguirà questo
  },
  "dependencies": {
    "express": "^4.17.1"               // Express come dipendenza principale
  }
}
# 📌 Questo serve per:

Installare le dipendenze

Definire come lanciare il server (npm start)

Identificare il progetto Node.js

# passo 4
Una volta creato package.json, esegui:

npm install
Questo: Scarica la libreria express da internet

Crea una cartella node_modules/ con tutto il necessario

Crea anche il file package-lock.json

## 📁 A questo punto nella cartella hello-docker hai:
app.js
package.json
package-lock.json
node_modules/

# passo 5

Crea il file:
nano Dockerfile

Incolla:

# Usa un'immagine base di Node.js
FROM node:14

# Imposta la directory di lavoro all'interno del container
WORKDIR /usr/src/app

# Copia i file di configurazione del progetto (package.json e package-lock.json)
COPY package*.json ./

# Installa le dipendenze (npm install)
RUN npm install

# Copia tutto il resto (compreso app.js)
COPY . .

# Esponi la porta 3000 per accedere all'app
EXPOSE 3000

# Comando da eseguire quando parte il container
CMD ["npm", "start"]

# 🔍 Spiegazione dettagliata:

FROM node:14: Usa un'immagine con Node.js 14 preinstallato
WORKDIR:	Entra nella cartella /usr/src/app dentro il container
COPY package*	Copia package.json e package-lock.json per installare prima le deps
RUN npm install:	Installa le dipendenze (express, ecc.)
COPY:	Copia tutto il resto del progetto (compreso app.js)
EXPOSE 3000:	Dichiara che il container userà la porta 3000
CMD:	Quando parte il container, esegue npm start, cioè node app.js

# passo 6
Ora che hai tutto, esegui:

sudo docker build -t hello-docker 

✅ Questo comando:
Usa il Dockerfile nella cartella corrente (.)
Crea un'immagine Docker chiamata hello-docker
Vedrai Docker “costruire i layer” uno per uno.

# passo 7
PASSO 7 – Avvia il container
Lancia il container con:

sudo docker run -d -p 3000:3000 hello-docker
ecco i significati:
-d	Avvia in background (detached mode)
-p	Mappa porta host:container
3000:3000	Mappa porta 3000 del tuo PC/VM con quella del container
hello-docker	Nome dell'immagine da cui creare il container

# passo 8
🌐 PASSO 8 – Accedi all'app dal browser
Se lavori in locale:

http://localhost:3000

Se sei su una VM:

http://<IP_pubblico_della_VM>:3000

# RISULTATO:
Se tutto è ok, nel browser vedrai:
Hello, World!
