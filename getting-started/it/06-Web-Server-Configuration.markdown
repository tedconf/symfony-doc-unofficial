
Configurazione del Web Server
=============================

Modalità poco pulita e poco sicura
----------------------------------

Nel capitolo precedente, è stata creata una directory che ospita il progetto.
Se essa e' stata creata all'interno della directory root del server web,
è già possibile accedere al progetto stesso tramite un browser web.

Ovviamente, non essendoci una configurazione specifica, quanto fatto finora è molto facile 
e veloce da impostare, ma se si provasse per esempio ad accedere al file `config/databases.yml` 
tramite il browser web si comprenderebbero le conseguenze negative di tale attitudine.
Se un utente venisse a conoscenza che il sito web in questione e' sviluppato 
con Symfony, avrebbe facilmente accesso a file che contengono informazioni sensibili.


**Mai utilizzare questa tipologia di configurazione su un server di produzione**,
si invita alla lettura della sezione successiva per comprendere come configurare
correttamente il proprio web server. 


Modalita' sicura
----------------

In ambito web è buona prassi posizionare all'interno della directory root del
web server solo i file che necessitano l'accesso da parete del browser web come
ad esempio i fogli di stile, Javascript e le immagini.
Come opzione predefinita, si raccomanda di posizionare queste tipologie di file all'interno
della directory `web/`.


All'interno di questa directory sono presenti alcune sotto directory  delle varie 
risorse web (`css/` and `images/`) e i due file front controller.
Quest'ultimi sono gli unici file PHP che devono essere posizionati all'interno 
della directory web.Tutti gli altri file PHP devono essere nascosti, non raggiungibili,
dal browser web, cio' rappresenta una buona soluzione per la sicurezza dell'applicativo.


### Configurazione del Web Server
È giunto il momento di cambiare la configurazione di Apache in modo
da rendere accessibile esternamente il nuovo progetto.

Localizzare e aprire il file di configurazione `httpd.conf` e aggiungere le seguenti
rige alla fine dello stesso:

   
    #Essere sicuri che la riga seguente sia presente un'unica volta all'interno 
    #del file di configurazione
    NameVirtualHost 127.0.0.1:8080

    #Configurazione specifica del progetto
    Listen 127.0.0.1:8080

    <VirtualHost 127.0.0.1:8080>
      DocumentRoot "/home/sfproject/web"
      DirectoryIndex index.php
      <Directory "/home/sfproject/web">
        AllowOverride All
        Allow from All
      </Directory>

      Alias /sf /home/sfproject/lib/vendor/symfony/data/web/sf
      <Directory "/home/sfproject/lib/vendor/symfony/data/web/sf">
        AllowOverride All
        Allow from All
      </Directory>
    </VirtualHost>


>**NOTE**
>L'alias `/sf` permette l'accesso alle immagini e file javascript necessari
>alla visualizzazione delle pagine predefinite di symfony e alla web debug toolbar.
>
>Su Windows, bisogna rimpiazzare la riga che definisce l'`Alias` con
>
>     Alias /sf "c:\dev\sfproject\lib\vendor\symfony\data\web\sf"
>
>e `/home/sfproject/web` dovrebbe essere rimpiazzato con:
>
>     c:\dev\sfproject\web


La configurazione appena descritta mette in ascolto Apache sulla porta `8080`,
quindi il sito web sara' raggiungibile al seguente URL:

    http://localhost:8080/

E' possibile sostituire `8080` con qualsiasi altro numero ma e' preferibile utilizzare
numeri superiori a `1024` in quanto non richiedono privilegi di amministratore.

>**SIDEBAR**
>Configurazione di un dominio dedicato
>
>Nel caso in cui si e' amministratori del server stesso, e' meglio 
>creare e configurare dei virtual host piuttosto che aggiungere una nuova porta 
>ogni qualvolta si voglia iniziare un progetto.Invece di scegliere una porta e
>aggiungere la direttiva `Listen`, scegliere un dominio e aggiungere la direttiva
>`ServerName`:
>
>     #Configurazione del progetto
>     <VirtualHost 127.0.0.1:80>
>       ServerName sfproject.localhost
>       <!-- same configuration as before -->
>     </VirtualHost>
>
>
>Il dominio `sfproject.localhost` utilizzato nella configurazione di Apache
>deve essere dichiarato localmente.Su un sistema Linux, modficare il file `/etc/hosts`.
>In un sistema Windows invece il file si trova nella directory `C:\WINDOWS\system32\drivers\etc\`.
>
>Aggiungere la riga seguente:
>
>     127.0.0.1 sfproject.localhost


### Testare la Nuova Configurazione 

Riavviare Apache e controllare che sia possibile l'accesso alla nuova applicazione
aprendo un browser web e digitando `http://localhost:8080/index.php/` oppure
`http://sfproject.localhost/index.php/`, cio' dipende dalla configurazione scelta
nella precedente sezione.



![Congratulazioni](http://www.symfony-project.org/images/jobeet/1_2/01/congratulations.png)
>**TIP**
>Se il modulo `mod_rewrite` di Apache e' installato e attivo, e' possibile rimuovere
>`index.php/` dall'URL.Questo e' possibile grazie alle regole di riscrittura presenti nel file
>`web/.htaccess`.


E' possibile accedere all'applicativo in modalita' di ambiente di sviluppo(vedere la 
sezione successiva per maggiori informazioni sui diversi ambienti).Digitare la
seguente URL:

    http://sfproject.localhost/frontend_dev.php/

Dovrebbe essere visibile nell'angolo in alto a destra la web debug toolbar, con 
delle piccole icone rese visibili, se tutto e' stato configurato correttamente,
 grazie all' `sf/` alias.


![web debug toolbar](http://www.symfony-project.org/images/jobeet/1_2/01/web_debug_toolbar.png)

>**NOTE**
>
>La creazione e configurazione del progetto è leggermente differente se si volesse
>utilizzare symfony in combinazione con IIS server in ambiente Windows.
>Le istruzioni di configurazioni sono disponibili nel 
>[tutorial relativo](http://www.symfony-project.com/cookbook/1_0/web_server_iis).
