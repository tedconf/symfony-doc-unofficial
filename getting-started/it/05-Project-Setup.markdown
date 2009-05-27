Preparazione del progetto
=========================

In symfony, le **applicazioni** che condividono lo stesso modello dei dati
sono raggruppate in **progetti**. Per la maggior parte dei progetti si
avranno due diverse applicazioni: un frontend ed un backend.

### Creazione del progetto

Dalla cartella `sfproject/`, eseguire il task di symfony `generate:project`
per creare effettivamente il progetto symfony:

    $ php lib/vendor/symfony/data/bin/symfony generate:project PROJECT_NAME

Su Windows:

    c:\> php lib\vendor\symfony\data\bin\symfony generate:project PROJECT_NAME

Il task `generate:project` genera la struttura predefinita di cartelle e di
file necessaria ad un progetto symfony:

 | Cartella    | Descrizione
 | ----------- | ----------------------------------
 | `apps/`     | Contiene tutte le applicazioni del progetto
 | `cache/`    | I file messi in cache dal framework
 | `config/`   | I file di configurazione del progetto
 | `lib/`      | Le classi e le librerie del progetto
 | `log/`      | I file di log del framework
 | `plugins/`  | I plugin installati
 | `test/`     | I file per i test unitari e funzionali
 | `web/`      | La cartella radice del web (vedi sotto)

>**NOTE**
>Per quale motivo symfony genera così tanti file? Uno dei maggiori
>benefici che derivano dall'uso di un framework full-stack è quello
>della standardizzazione dello sviluppo. Grazie alla struttura
>predefinita di file e cartelle di symfony, ogni sviluppatore che
>conosca symfony può occuparsi della manutenzione di qualsiasi
>progetto symfony. In pochi minuti, egli avrà la possibilità
>di analizzare il codice, sistemare i bug e aggiungere nuove
>caratteristiche.

Il task `generate:project` crea anche un collegamento `symfony` nella
cartella radice del progetto, per accorciare il numero di caratteri
da scrivere quando si esegue un task.

Quindi, d'ora in poi, invece di usare il percorso completo a symfony,
si può usare il collegamento `symfony`.

### Creazione di un'applicazione

Creiamo ora l'applicazione frontend, eseguendo il task `generate:app`:

    $ php symfony generate:app --escaping-strategy=on
      ➥ --csrf-secret=UniqueSecret frontend

>**TIP**
>Essendo il collegamento a symfony un file eseguibile, gli utenti Unix
>possono d'ora in poi sostituire tutte le occorrenze di '`php symfony`'
>con '`./symfony`'
>
>Su Windows si può copiare il file '`symfony.bat`' all'interno del
>progetto ed usare '`symfony`' invece di '`php symfony`':
>
>     c:\> copy lib\vendor\symfony\data\bin\symfony.bat .

Basandosi sul nome dell'applicazione fornito come *parametro*, il task
`generate:app` crea la struttura di cartelle predefinita necessaria
per l'applicazione, sotto la cartella `apps/frontend/`:

 | Cartella     | Descrizione
 | ------------ | -------------------------------------
 | `config/`    | I file di configurazione dell'applicazione
 | `lib/`       | Le librerie e le classi dell'applicazione
 | `modules/`   | Il codice dell'applicazione (MVC)
 | `templates/` | I file dei template globali

>**TIP**
>Richiamando il task `generate:app` task, abbiamo usato anche due *opzioni*
>relative alla sicurezza:
>
>  * `--escaping-strategy`: Abilita l'escape dell'output, per prevenire attacchi XSS
>  * `--csrf-secret`: Abilita i token di sessione nei form, per prevenire attacchi CSRF
>
>Passando queste due opzioni facoltative al task, abbiamo messo in sicurezza
>lo sviluppo futuro da due delle più diffuse vulnerabilità del web. Già, symfony
>si prende cura della sicurezza per noi.
>
>Se si ignora cosa siano 
>[XSS](http://it.wikipedia.org/wiki/Cross-site_scripting) e
>[CSRF](http://it.wikipedia.org/wiki/CSRF), sarebbe meglio spendere un po' di tempo
>per sapere di più su queste vulnerabilità.

### Permessi sulla struttura di cartelle

Prima di provare ad accedere al nuovo progetto, occorre impostare i
permessi di scrittura sulle cartelle `cache/` e `log/` ai livelli
appropriati, in modo tale che il server web possa scriverci dentro:

    $ chmod 777 cache/ log/

>**SIDEBAR**
>Consigli per chi usa uno strumento di revisione del codice
>
>symfony scrive solamente in due cartelle di un progetto symfony,
>`cache/` e `log/`. Il contenuto di queste due cartelle dovrebbe essere
>ignorato dagli strumenti di revisione del codice (ad esempio
>utilizzando la proprietà `svn:ignore`, se si usa Subversion).

### Configurare il database

Una delle prime cose da fare è quella di configurare la connessione al
database del progetto. Il framework symfony supporta tutti i database
supportati da [PDO]((http://www.php.net/PDO)) (MySQL, PostgreSQL,
SQLite, Oracle, MSSQL, ...). Appoggiandosi a PDO, symfony è distribuito
con due strumenti ORM:Propel and Doctrine. Propel è quello predefinito,
ma passare a Doctrine è molto facile (si veda la prossima sezione per
maggiori informazioni).

La configurazione del database è semplificata dall'uso del task `configure:database`:

    $ php symfony configure:database "mysql:host=localhost;dbname=dbname" root mYsEcret

Il task `configure:database` accetta tre parametri: il
[~DSN di PDO~](http://www.php.net/manual/it/pdo.drivers.php), il nome utente e
la password per accedere al database. Se non si ha bisogno di una password per
accedere al database sul server di sviluppo, basta omettere il terzo parametro.

### Passare a Doctrine

Se si sceglie di usare Doctrine al posto di Propel, occorre innanzitutto abilitare
`sfDoctrinePlugin` e disabilitare `sfPropelPlugin`. L'operazione può essere
eseguita modificando il seguente codice in `config/ProjectConfiguration.class.php`:

    [php]
    public function setup()
    {
      $this->enableAllPluginsExcept(array('sf#PropelPlugin', 'sfCompat10Plugin'));
    }

Dopo tali modifiche, lanciare i seguenti comandi:

    $ php symfony plugin:publish-assets
    $ php symfony cc
    $ rm web/sfPropelPlugin
    $ rm config/propel.ini
    $ rm config/schema.yml
    $ rm config/databases.yml

Quindi, eseguire i seguenti comandi per configurare un database per Doctrine:

    $ php symfony configure:database --name=doctrine --class=sfDoctrineDatabase "mysql:host=localhost;dbname=jobeet" root mYsEcret
