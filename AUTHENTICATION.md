# Account King Agent

L’interfaccia e il collegamento a Firebase Authentication sono implementati. `firebase-config.json` contiene la configurazione pubblica dell’app King Agent Web; Email/Password e Google sono abilitati nella console.

Progetto: `king-agent-c4f1a`, piano Spark. Nome pubblico Google: King Agent. Email di assistenza autorizzata dal proprietario durante la configurazione. Password minima: 8 caratteri.

## Configurazione e ricostruzione

Il progetto attuale è già configurato. I passaggi seguenti servono per una nuova installazione o per cambiare progetto.

1. Aprire https://console.firebase.google.com/ e il progetto King Agent.
2. Registrare un’app web e copiare `apiKey`, `authDomain`, `projectId`, `appId` in `firebase-config.json`. Questi sono parametri pubblici dell’app; non inserire chiavi di account di servizio o credenziali amministrative.
3. In Authentication → Metodo di accesso, abilitare Email/Password e Google. Per Google impostare il nome pubblico King Agent e l’email di assistenza del proprietario.
4. Nei domini autorizzati aggiungere `kazael96.github.io`. Per le verifiche locali aggiungere `127.0.0.1` e `localhost`.
5. Configurare una password minima di 8 caratteri nella policy del progetto. L’interfaccia legge e verifica la policy tramite l’SDK.
6. Eseguire `node work/build-account.cjs`, `node work/build-navigation.cjs`, `node work/build-requests.cjs`, `node work/build-mobile.cjs`, poi `node work/build-king.cjs` dalla radice del workspace.
7. Pubblicare `index.html`, `style.css` e `game.js` su GitHub Pages.
8. Verificare sul sito pubblico creazione di un account di prova, verifica email, uscita, accesso email, recupero password, accesso Google, ritorno dopo ricaricamento e annullamento del popup. Usare un indirizzo di prova controllato dal proprietario, non credenziali inventate di persone reali.

## Comportamento

- L’accesso è facoltativo e disponibile nella home e nelle impostazioni.
- Le password vengono inviate a Firebase Authentication; il gioco non le registra in localStorage, nei salvataggi o nei log.
- Google usa `signInWithPopup`, evitando un redirect tra GitHub Pages e il dominio Firebase. Il browser deve consentire il popup.
- La sessione viene mantenuta dall’SDK Firebase.
- Le carriere esistenti restano locali. Questa fase non aggiunge sincronizzazione cloud né separazione dei salvataggi per account. Non usare il login come promessa di recupero della partita su un altro dispositivo.
- Senza configurazione il modulo non raccoglie credenziali e spiega che l’accesso non è ancora attivo.

## Verifiche

`node work/verify-account.cjs` verifica con SDK simulato il blocco senza configurazione, la chiamata di accesso email, il recupero, la policy password e l’annullamento Google. Non sostituisce la verifica con il servizio reale.

Verificati anche il caricamento reale dell’SDK e la risposta di Firebase a un tentativo con credenziali di prova non valide. Creazione, verifica email e accesso riuscito con Google richiedono una prova con un account controllato dall’utente.

Documentazione: https://firebase.google.com/docs/auth/web/password-auth · https://firebase.google.com/docs/auth/web/google-signin · https://firebase.google.com/docs/auth/web/manage-users
## Carriere nel cloud

Firestore Standard è configurato nel database (default), europe-west8, progetto king-agent-c4f1a, piano Spark. Le regole pubblicate consentono lettura e scrittura solo in careers/{uid} al relativo utente autenticato; eliminazione e altri percorsi non sono autorizzati. Sono verificati schema, dimensione, timestamp server e incremento della revisione.

La sincronizzazione è facoltativa e si attiva nelle impostazioni dopo l’accesso. Non avviene alcuna sovrascrittura automatica di archivi differenti alla prima connessione. Ogni conflitto richiede una scelta e conserva una copia precedente nel browser. I test automatici simulano transazioni, conflitti e cambio account; la prova completa tra due dispositivi reali richiede accesso allo stesso account su entrambi.
