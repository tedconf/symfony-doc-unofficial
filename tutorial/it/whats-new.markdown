Che c'è di nuovo in symfony 1.3?
================================

Questo tutorial è una rapida introduzione tecnica a symfony 1.3.
È pensata per sviluppatori che hanno già lavorato con symfony 1.2 e
che vogliono conoscere in poco tempo le nuove caratteristiche di symfony 1.3.

Prima di tutto, si prega di notare che symfony 1.3 è compatibile con PHP 5.2.4
o successivi.

Se si vuole aggiornare dalla versione 1.2, si prega di leggere il file
[UPGRADE](http://www.symfony-project.org/tutorial/1_3/it/upgrade) che si trova
nella distribuzione di symfony.
Vi si troveranno tutte le informazioni necessarie per aggiornare senza
problemi i propri progetti a symfony 1.3.

Mailer
------

Da symfony 1.3, c'è un nuovo mailer predefinito, basato su SwiftMailer 4.1.

L'invio di messaggi email è facile, basta usare il metodo `composeAndSend()`
in un'azione:

    [php]
    $this->getMailer()->composeAndSend('from@example.com', 'to@example.com', 'Oggetto', 'Corpo');

Se si ha bisogno di maggiore flessibilità, si può anche usare il metodo
`compose()` ed inviare in seguito. Ecco ad esempio come aggiungere un allegato
al messaggio:

    [php]
    $message = $this->getMailer()->
      compose('from@example.com', 'to@example.com', 'Oggetto', 'Corpo')->
      attach(Swift_Attachment::fromPath('/percorso/di/un/file.zip'))
    ;
    $this->getMailer()->send($message);

Essendo il mailer molto potente, si faccia riferimento alla documentazione per
maggiori informazioni.

Sicurezza
---------

Quando si crea una nuova applicazione col task `generate:app`, le impostazioni
di sicurezza sono ora abilitate in modo predefinito:

  * `escaping_strategy`: Il valore predefinito è ora `true` (può essere
    disabilitato con l'opzione `--escaping-strategy`).

  * `csrf_secret`: Viene generata una password casuale e quindi la protezione
    CSRF è abilitata in modo predefinito (può essere disabilitata con l'opzione
    `--csrf-secret`). Si raccomanda vivamente di modificare la password
    generata casualmente, modificando il file di configurazione `settings.yml`
    oppure usando l'opzione `--csrf-secret`.

Widget
------

### Etichette predefinite

Quando un'etichetta viene generata automaticamente in base al nome del campo,
il suffisso `_id` è ora rimosso:

  * `first_name` => First name (come prima)
  * `author_id` => Author (prima era "Author id")

### `sfWidgetFormInputText`

La classe `sfWidgetFormInput` è ora astratta. I campi input text sono ora
creati con la classe `sfWidgetFormInputText`. Questa modifica rende più
facile l'introspezione delle classi dei form.

### Widget I18n

Sono stati aggiunti i seguenti widget:

  * `sfWidgetFormI18nChoiceLanguage`
  * `sfWidgetFormI18nChoiceCurrency`
  * `sfWidgetFormI18nChoiceCountry`
  * `sfWidgetFormI18nChoiceTimezone`

I primi tre sostituiscono i widget `sfWidgetFormI18nSelectLanguage`,
`sfWidgetFormI18nSelectCurrency` e `sfWidgetFormI18nSelectCountry`, che ora
sono deprecati.

### Interfaccia Fluida

I widget ora implementano un'interfaccia fluida per i seguenti metodi:

  * `sfWidgetForm`: `setDefault()`, `setLabel()`, `setIdFormat()`,
    `setHidden()`

  * `sfWidget`: `addRequiredOption()`, `addOption()`, `setOption()`,
    `setOptions()`, `setAttribute()`, `setAttributes()`

  * `sfWidgetFormSchema`: `setDefault()`, `setDefaults()`,
    `addFormFormatter()`, `setFormFormatterName()`, `setNameFormat()`,
    `setLabels()`, `setLabel()`, `setHelps()`, `setHelp()`, `setParent()`

  * `sfWidgetFormSchemaDecorator`: `addFormFormatter()`,
    `setFormFormatterName()`, `setNameFormat()`, `setLabels()`, `setHelps()`,
    `setHelp()`, `setParent()`, `setPositions()`

Validatori
----------

### `sfValidatorRegex`

`sfValidatorRegex` ha una nuova opzione `must_match`. Se impostata a `false`,
l'espressione regolare non deve essere soddisfatta per far sì che il validatore
passi.

L'opzione `pattern` di `sfValidatorRegex` può ora essere un'istanza di
`sfCallable` che restituisca un'espressione regolare quando richiamata.

### `sfValidatorUrl`

`sfValidatorUrl` ha una nuova opzione `protocols`. Questo consente di specificare
quali protocolli consentire:

    [php]
    $validator = new sfValidatorUrl(array('protocols' => array('http', 'https')));

I seguenti protocolli sono consentiti in modo predefinito:

 * `http`
 * `https`
 * `ftp`
 * `ftps`

### `sfValidatorSchemaCompare`

La classe `sfValidatorSchemaCompare` ha ora due nuovi comparatori:

 * `IDENTICAL`, che equivale a `===`;
 * `NOT_IDENTICAL`, che equivale a `!==`;

### `sfValidatorChoice`, `sfValidatorPropelChoice`, `sfValidatorDoctrineChoice`

I validatori `sfValidatorChoice`, `sfValidatorPropelChoice`,
`sfValidatorDoctrineChoice` hanno ora due nuove opzioni abilitate solo se
l'opzione `multiple` è `true`:

 * `min` Il numero minimo di valori che occorre selezionare
 * `max` Il numero massimo di valori che occorre selezionare

### Validatori I18n

Sono stati aggiunti i seguenti validatori:

 * `sfValidatorI18nTimezone`

### Messaggi di Errore Predefiniti

Si possono ora definire dei messaggi di errore predefiniti in modo globale,
usando il metodo `sfForm::setDefaultMessage()`:

    [php]
    sfValidatorBase::setDefaultMessage('required', 'Questo campo è obbligatorio.');

Il codice precedente sovrascriverà il messaggio predefinito 'Required.' per
tutti i validatori. Si noti che i messaggi predefiniti devono essere definiti
prima che qualsiasi validatore sia creato (la classe di configurazione è
un buon posto per farlo).

>**NOTE**
>I metodi `setRequiredMessage()` e `setInvalidMessage()` sono deprecati e
>richiamano il nuovo metodo `setDefaultMessage()`.

Quando symfony mostra un errore, il messaggio di errore da usare viene
determinato nel modo seguente:

 * Symfony cerca un messaggio passato quando il validatore è stato creato
   (tramite il secondo parametro al costruttore del validatore);

 * Se non definito, cerca il messaggio predefinito definito col metodo
   `setDefaultMessage()`;

 * Se non definito, usa il messaggio predefinito dal validatore stesso
   (quando il messaggio è stato aggiunto col metodo `addMessage()`).

### Intefaccia Fluida

I validatori ora implementano un'interfaccia fluida per i seguenti metodi:

  * `sfValidatorSchema`: `setPreValidator()`, `setPostValidator()`

  * `sfValidatorErrorSchema`: `addError()`, `addErrors()`

  * `sfValidatorBase`: `addMessage()`, `setMessage()`, `setMessages()`,
    `addOption()`, `setOption()`, `setOptions()`, `addRequiredOption()`

sfToolkit
---------

Il metodo `getTmpDir()` è stato deprecato e non viene più usato nelle classi
principali di symfony. Si può sostituire l'utilizzo di tale metodo con la funzione
di libreria di PHP `sys_get_temp_dir()`. Ora `getTmpDir()` non è altro che un
proxy a tale funzione.

Questo metodo sarà rimosso in symfony 1.4.

Form
----

### `sfForm::useFields()`

Il nuovo metodo `sfForm::useFields()` rimuove da un form tutti i campi non
nascosti, tranne quelli passati come parametro. A volte è più semplice
esplicitare i campi da mantenere piuttosto che eliminare tutti quelli non
necessari. Ad esempio, quando si aggiungono nuovi campi ad un form di base,
essi non appariranno nel form a meno che non siano esplicitamente aggiunti
(si pensi ad un modello di form in cui si aggiungono nuove colonne alla
tabella relativa).

    [php]
    class ArticleForm extends BaseArticleForm
    {
      public function configure()
      {
        $this->useFields(array('title', 'content'));
      }
    }

Di default, l'array di campi è usato anche per cambiare l'ordine
dei campi. Si può passare `false` come secondo parametro di `useFields()`
per disabilitare il riordinamento automatico.

### `sfFormSymfony`

La nuova classe `sfFormSymfony` introduce il dispatcher di eventi per i
form. Si può accedere al dispatcher da dentro le proprie classi di form tramite
`self::$dispatcher`. I seguenti eventi di form sono ora notificati da symfony:

  * `form.post_configure`:   Questo evento è notificato dopo che ogni form è
                             configurato
  * `form.filter_values`:    Questo evento filtra i parametri ed i file fusi e
                             quelli grezzi subito prima del bind
  * `form.validation_error`: Questo evento è notificato ogni volta che un form
                             fallisce
  * `form.method_not_found`: Questo evento è notificato ogni volta che un metodo
                             sconosciuto viene richiamato

### `BaseForm`

Ogni nuovo progetto in symfony 1.3 include una classe `BaseForm` che si può
usare per estendere il componente Form o per aggiungere funzionalità
specifiche a livello di progetto. I form generati automaticamente da
`sfDoctrinePlugin` e `sfPropelPlugin` estendono questa classe. Se si creano
ulteriori classi di form, queste dovrebbero ora estendere `BaseForm` e
non `sfForm`.

### `sfForm::doBind()`

La pulizia dei parametri grezzi è stata isolata in un metodo amichevole per
lo sviluppatore, `->doBind()`, che riceve l'array fuso di parametri e file da
`->bind()`.

### `sfForm(Doctrine|Propel)::doUpdateObject()`

Le classi di form di Doctrine e Propel ora includono un metodo amichevole
per gli sviluppatori, `->doUpdateObject()`. Questo metodo riceve da
`->updateObject()` un array di valori che sono stati già processati da
`->processValues()`.

### `sfForm::enableLocalCSRFProtection()` e `sfForm::disableLocalCSRFProtection()`

Usando i metodi `sfForm::enableLocalCSRFProtection()` e
`sfForm::disableLocalCSRFProtection()`, si può ora configurare facilmente
la protezione CSRF dal metodo `configure()` delle proprie classi di form.

Per disabilitare la protezione CSRF per un form, aggiungere la seguente riga
nel suo metodo `configure()`:

    [php]
    $this->disableLocalCSRFProtection();

Richiamando `disableLocalCSRFProtection()`, la protezione CSRF sarà
disabilitata, anche se si passa una stringa segreta CSRF all'istanza del form.

### Interfaccia Fluida

Alcuni metodi di `sfForm` ora implementano un'interfaccia fluida:
`addCSRFProtection()`, `setValidators()`, `setValidator()`, `setValidatorSchema()`,
`setWidgets()`, `setWidget()`, `setWidgetSchema()`, `setOption()`, `setDefault()` e
`setDefaults()`.

Autoloader
----------

Tutti gli autoloader di symfony ora trascurano le differenze tra maiuscolo e
minuscole, seguendo lo stesso comportamento di PHP.

### `sfAutoloadAgain` (SPERIMENTALE)

Un autoloader speciale, pensato per il debug, è stato aggiunto. La nuova classe
`sfAutoloadAgain` ricaricherà l'autoloader predefinito di symfony e cercherà
le classi in questione. L'effetto netto è che non si avrà più bisogno di
eseguire `symfony cc` dopo l'aggiunta di una nuova classe al progetto.

Test
----

### Accelerazione dei Test

Quando si hanno molti test, lanciarli tutti dopo ogni modifica può portar via
molto tempo, specialmente quando alcuni test falliscono. Questo perché
ogni volta che si sistema un test, si dovrebbe rieseguire da capo l'intera lista
di test per assicurarsi di non aver creato problemi altrove. Ma finché
i test falliti non sono sistemati, non serve eseguire di nuovo tutti gli
altri test. Da symfony 1.3, i task `test:all` e `symfony:test` hanno un'opzione
`--only-failed` (scorciatoia: `-f`), che forza i task ad eseguire nuovamente
solo i test falliti durante la precedente esecuzione:

    $ php symfony test:all --only-failed

Ecco come funziona: la prima volta, tutti i test sono eseguiti, come sempre.
Ma dalle seguenti esecuzioni, solo i test che hanno fallito l'ultima volta
vengono eseguiti. Man mano che si sistema il proprio codice, alcuni test
passeranno e saranno quindi rimossi dalle esecuzioni successive. Quando
tutti i test passano di nuovo, tutti i test vengono eseguiti... si può
quindi ripetere a piacere.

### Test Funzionali

Quando una richiesta genera un'eccezione, il metodo `debug()` del tester della
risposta ora manda in output una rappresentazione testuale leggibile
dell'eccezione, invece del normale output in HTML. Questo rende il debug più
facile.

`sfTesterResponse` ha un nuovo metodo `matches()`, che esegue un'espressione
regolare sull'intero contenuto della risposta. Questo è di grande aiuto per
risposte che non si basano su XML, in cui `checkElement()` non è utilizzabile.
Inoltre sostitusce il metodo `contains()`, meno potente:

    [php]
    $browser->with('response')->begin()->
      matches('/I have \d+ apples/')->    // accetta un'espressione regolare come parametro
      matches('!/I have \d+ apples/')->   // un punto esclamativo iniziale nega l'espressione regolare
      matches('!/I have \d+ apples/i')->  // si possono usare anche i modificatori
    end();

### Output XML Compatibile con JUnit

I task dei test ora possono inviare in output un file XML compatibile con
JUnit, usando l'opzione `--xml`:

    $ php symfony test:all --xml=log.xml

### Debug Facile

Per facilitare il debug quando l'insieme di test riporta dei test falliti,
si può ora passare l'opzione `--trace` per avere un output dettagliato
dei fallimenti:

    $ php symfony test:all -t

### Colorazione dell'Output di Lime

Da symfony 1.3, lime si comporta bene riguardo alla colorazione. Questo vuol
dire che si può omettere quasi sempre il secondo parametro del costruttore
`lime_test`:

    [php]
    $t = new lime_test(1);

### `sfTesterResponse::checkForm()`

Il tester della risposta ora include un metodo per verificare facilmente che
tutti i campi in un form siano stati resi nella risposta:

    [php]
    $browser->with('response')->begin()->
      checkForm('ArticleForm')->
    end();

O, se si preferisce, si può passare un oggetto form:

    [php]
    $browser->with('response')->begin()->
      checkForm($browser->getArticleForm())->
    end();

Se la risposta include form multipli, si può fornire un selettore CSS per
puntare esattamente alla porzione di DOM da testare:

    [php]
    $browser->with('response')->begin()->
      checkForm('ArticleForm', '#articleForm')->
    end();

### Ascoltare `context.load_factories`

Si possono ora aggiungere ai propri test funzionali degli ascoltatori per
l'evento `context.load_factories`. Questo non era possibile nelle
precedenti versioni di symfony.

    [php]
    $browser->addListener('context.load_factories', array($browser, 'listenForNewContext'));

Task
----

L'interfaccia a linea di comando di symfony ora cerca di rilevare l'ampiezza
della finestra del terminale e formattare le righe per adattarvisi. Se il
rilevamento non è possibile, la larghezza predefinita è di 78 colonne.

### `sfTask::askAndValidate()`

C'è un nuovo metodo `sfTask::askAndValidate()` per porre una domanda all'utente
e validare il suo input:

    [php]
    $anwser = $this->askAndValidate('Qual è il tuo indirizzo email?', new sfValidatorEmail());

Il metodo accetta anche un array di opzioni (si veda la documentazione delle API
per maggiori informazioni).

### `symfony:test`

Di tanto in tanto, gli sviluppatori hanno bisogno di eseguire l'insieme di test
di symfony, per verificare che symfony funzioni a dovere sulle loro
specifiche piattaforme. Finora, erano costretti a conoscere lo script
`prove.php`, distribuito con symfony, per farlo. Da symfony 1.3, c'è
un task `symfony:test`, che lancia l'insieme dei test di symfony dalla
linea di comando, come ogni altro task:

    $ php symfony symfony:test

Se si era soliti usare `php test/bin/prove.php`, si dovrebbe ora eseguire
il comando equivalente `php data/bin/symfony symfony:test`.

### `project:deploy`

Il task `project:deploy` è stato leggermente migliorato. Ora mostra la
progressione del trasferimento dei file in tempo reale, ma solo se viene
passata l'opzione `-t`. Altrimenti il task è silente, tranne ovviamente
per gli errori. Parlando di errori, l'output ora è su uno sfondo rosso,
per una migliore identificazione dei problemi.

### `generate:project`

Da symfony 1.3, Doctrine è l'ORM predefinito durante l'esecuzione del
task `generate:project`:

    $ php /path/to/symfony generate:project pippo

Per generare un progetto con Propel, usare l'opzione `--orm`n:

    $ php /path/to/symfony generate:project pippo --orm=Propel

Se non si vuole usare né Propel né Doctrine, si può passare `none`
all'opzione `--orm`:

    $ php /path/to/symfony generate:project pippo --orm=none

La nuova opzione `--installer` consente di passare uno script PHP che può
personalizzare ulteriormente il progetto appena creato. Lo script viene
eseguito nel contesto del task, quindi può usare tutti i suoi metodi. I
più utili sono i seguenti: `installDir()`, `runTask()`, `ask()`,
`askConfirmation()`, `askAndValidate()`, `reloadTasks()`, `enablePlugin()`
e `disablePlugin()`.

Si possono trovare maggiori informazioni in questo
[articolo](http://www.symfony-project.org/blog/2009/06/10/new-in-symfony-1-3-project-creation-customization)
del blog ufficiale di symfony.

### `sfFileSystem::execute()`

Il metodo `sfFileSystem::execute()` sostituisce il metodo `sfFileSystem::sh()`
con caratteristiche più potenti. Accetta dei callback per processare in
tempo reale gli output di `stdout` e `stderr`. Inoltre restituisce entrambi
gli output come array. Si può trovare un esempio del suo utilizzo nella classe
`sfProjectDeployTask`.

### `task.test.filter_test_files`

I task `test:*` ora filtrano i file di test tramite l'evento
`task.test.filter_test_files` prima di eseguirli. Questo evento include i
parametri `arguments` e `options.

### Miglioramenti a `sfTask::run()`

Si può ora passare un array associativo di parametri ed opzioni al task
`sfTask::run()`:

    [php]
    $task = new sfDoctrineConfigureDatabaseTask($this->dispatcher, $this->formatter);
    $task->run(array(
      'dsn' => 'mysql:dbname=mydb;host=localhost',
    ), array(
      'name' => 'master',
    ));

La versione precedente, che funziona ancora:

    [php]
    $task->run(array(
      'mysql:dbname=mydb;host=localhost',
    ), array(
      '--name=master',
    ));

### `sfBaseTask::setConfiguration()`

Richiamando un task che estende `sfBaseTask` da PHP, non serve più passare
le opzioni `--application` e `--env` a `->run()`. Invece, si può
semplicemente impostare l'oggetto configurazione direttemente, richiamando
`->setConfiguration()`.

    [php]
    $task = new sfDoctrineLoadDataTask($this->dispatcher, $this->formatter);
    $task->setConfiguration($this->configuration);
    $task->run();

La versione precedente, che funziona ancora:

    [php]
    $task = new sfDoctrineLoadDataTask($this->dispatcher, $this->formatter);
    $task->run(array(), array(
      '--application='.$options['application'],
      '--env='.$options['env'],
    ));

### `project:disable` e `project:enable`

Si può ora disabilitare o abilitare in massa un intero ambiente, usando
i task `project:disable` e `project:enable`:

    $ php symfony project:disable prod
    $ php symfony project:enable prod

Si può anche specificare quali applicazioni disabilitare nell'ambiente:

    $ php symfony project:disable prod frontend backend
    $ php symfony project:enable prod frontend backend

Questi task sono retrocompatibili con le loro precedenti firme:

    $ php symfony project:disable frontend prod
    $ php symfony project:enable frontend prod

### `help` e `list`

I task `help` e `list` possono ora mostrare le loro informazioni in XML:

    $ php symfony list --xml
    $ php symfony help test:all --xml

L'output è basato sul nuovo metodo `sfTask::asXml()`, che restituisce la
rappresentazione XML di uno oggetto task.

L'output XML è molto utile per strumenti di terze parti come gli IDE.

Eccezioni
---------

### Autoload

Quando viene lanciata un'eccezione durante un autoload, symfony ora la
cattura e mostra l'errore all'utente. Questo dovrebbe risolvere il problema
delle pagine bianche.

### Web Debug Toolbar

Se possibile, la web debug toolbar ora viene mostrata anche sulle pagine
delle eccezioni, nell'ambiente di sviluppo.

Propel
------

### `propel:insert-sql`

Prima che `propel:insert-sql` rimuova tutti i dati da un database, chiede una
conferma. Poiché questo task può rimuovere dati da molti database, ora mostra
il nome della connessione dei database coinvolti.

### Attributo di colonna `isPrimaryString`

Si può ora includere un attributo `isPrimaryString` nel file `schema.yml` e symfony
genererà un metodo `__toString()`, che restituisce il valore della colonna, nella
classe del modello.

    [yml]
    classes:
      Article:
        columns:
          id:     ~
          title:  { type: varchar(255), isPrimaryString: true }
          body:   { type: longvarchar }

Questa condifurazione creerà il seguente metodo in `BaseArticle`:

    [php]
    public function __toString()
    {
      return $this->getTitle();
    }

Routing
-------

### Richieste Predefinite

La richiesta predefinita `\d+` è ora applicata a `sfObjectRouteCollection`
solo quando l'opzione `column` è il predefinito `id`. Questo vuol dire
che non si ha bisogno di fornire una richiesta alternativa per colonne
non numeriche (come ad esempio `slug`).

CLI
---

### Colorazione dell'Otput 

Symfony prova ad indovinare se la console supporta i colori, quando si usano
gli strumenti della CLI. Ma a volte symfony si sbaglia; ad esempio quando si
usa Cygwin (poiché la colorazione è sempre spenta su Windows).

Da symfony 1.3, si può forzare l'uso di colori per l'output, passando l'opzione
globale `--color`.


TODO
