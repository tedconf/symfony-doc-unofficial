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


I18N
----

### Aggiornamento dei dati

I dati usati per tutte le operazioni I18N sono stati aggiornati dal
`progetto ICU`. Ora symfony ha circa 330 file di localizzazione, rispetto
ai circa 70 di symfony 1.2. Si noti che i dati aggiornati potrebbero essere
leggermente diversi da quelli precedenti, quindi ad esempio un test che
verifichi il decimo elemento in una lista di lingue potrebbe fallire.

### Ordinamento in base alla localizzazione dell'utente

Tutti gli ordinamenti sui dati localizzati ora sono eseguiti in base
alla localizzazione stessa. A tale scopo si può usare `sfCultureInfo->sortArray()`.

Plugin
------

Prima di symfony 1.3, tutti i plugin erano abilitati in modo predefinito, eccetto
`sfDoctrinePlugin` e `sfCompat10Plugin`:

    [php]
    class ProjectConfiguration extends sfProjectConfiguration
    {
      public function setup()
      {
        // for compatibility / remove and enable only the plugins you want
        $this->enableAllPluginsExcept(array('sfDoctrinePlugin', 'sfCompat10Plugin'));
      }
    }

Per i nuovi progetti creati con symfony 1.3, i plugin devono essere
esplicitamente abilitati nella classe `ProjectConfiguration` per poter
essere usati:

    [php]
    class ProjectConfiguration extends sfProjectConfiguration
    {
      public function setup()
      {
        $this->enablePlugins('sfDoctrinePlugin');
      }
    }

Il task `plugin:install` abilita automaticamente i plugin che installa (e
`plugin:uninstall` li disabilita). Se si installa un plugin tramite
Subversion, occorre abilitarlo a mano.

Se si vuole usare un plugin del core, come `sfProtoculousPlugin` o
`sfCompat10Plugin`, basta aggiungere il corrispondente comando `enablePlugins()`
nella classe `ProjectConfiguration`.

>**NOTE**
>Se si aggiorna un progetto dalla versione 1.2, il vecchio comportamento
>sarà ancora attivo, perché il task di aggiornamento non modifica il file
>`ProjectConfiguration`. Il cambio di comportamento è solo per i nuovi
>progetti basati su symfony 1.3.

### `sfPluginConfiguration::connectTests()`

Si possono connettere i test di un plugin ai task `test:*` richiamando
il metodo `->connectTests()` della configurazione del plugin stesso, nel
nuovo metodo `setupPlugins()`:

    [php]
    class ProjectConfiguration extends sfProjectConfiguration
    {
      public function setupPlugins()
      {
        $this->pluginConfigurations['sfExamplePlugin']->connectTests();
      }
    }

Impostazioni
------------

### `sf_file_link_format`

Symfony 1.3 formatta i percorsi dei file come link cliccabili, ovunque sia
possibile (ad esempio nel template dell'eccezione). A questo scopo viene
usato `sf_file_link_format`, se impostato, altrimenti symfony cercherà
il valore `xdebug.file_link_format` nella configurazione di PHP.

Ad esempio, se si vogliono aprire i file in TextMate, si aggiungano le
seguenti righe in `settings.yml`:

    [yml]
    all:
      .settings:
        file_link_format: txmt://open?url=file://%f&line=%l

Il segnaposto `%f` sarà sostituito con il percorso assoluto del file ed il
segnamposto `%l` sarà sostituito dal numero di riga.

Integrazione con Doctrine
-------------------------

### Generazione delle Classi dei Form

È ora possibile specificare delle opzioni aggiuntive per symfony nei file YAML
degli schema di Doctrine. Abbiamo aggiunto alcune opzioni per disabilitare la
generazione di classi di form e di filtri.

Ad esempio, in un tipico modello di riferimento molti-a-molti, non si ha
bisogno di generare classi di form né di filtri. Quindi si può fare in
nel modo seguente.

    UserGroup:
      options:
        symfony:
          form: false
          filter: false
      columns:
        user_id:
          type: integer
          primary: true
        group_id:
          type: integer
          primary: true

### Ereditarietà delle Classi dei Form

Quando si generano form dai modelli, i modelli contengono ereditarietà.
Le classi figlie generate rispetterano l'ereditarietà e genereranno
form che seguono la stessa struttura di ereditarietà.

### Nuovi Task

Abbiamo introdotto alcuni nuovi task che aiutano nello sviluppo con Doctrine.

#### Create Model Tables

Si possono ora creare individualmente le tabelle per uno specifico array di
modelli. Le tabelle saranno eliminate e create nuovamente. Questo è utile
se si stanno sviluppando nuovi modelli su un progetto/database esistente e
non si vuole cancellare l'intero database, ma solo ricostruire un
sottoinsieme di tabelle.

    $ php symfony doctrine:create-model-tables Model1 Model2 Model3

#### Delete Model Files

Spesso si cambiano modelli, li si rinomina, si rimuovono quelli non più
usati e via dicendo nello schema YAML. Quando lo si fa, si resta con 
modelli, form e filtri orfani. Ora si possono pulire manualmente i file
generati da un modello usando il task `doctrine:delete-model-files`.

    $ php symfony doctrine:delete-model-files ModelName

Il task troverà tutti i file generati e li mostrerà prima di chiedere una
conferma di cancellazione.

#### Clean Model Files

Si può automatizzare il processo appena visto e trovare quali modelli
esistono sul disco ma non nello schema YAML, usando il task
`doctrine:clean-model-files`.

    $ php symfony doctrine:clean-model-files

Il task confronterà i file di schema YAML con i modelli ed i file che
sono stati generati e determinerà quali rimuovere. Questi modelli
sono passati al task `doctrine:delete-model-files`. Una conferma
sarà richiesta prima della cancellazione effettiva.

#### Reload Data

Spesso si ha l'esigenza di azzerare il database e ricaricare le fixture.
Il task  `doctrine:build-all-reload` si occupa di questo, ma fa anche
un sacco di altre cose (generazione di modelli, form, filtri, ecc.) e
questo, in un grosso progetto, può portar via del tempo. Ora si può
usare il task `doctrine:reload-data`.

Il seguente comando:

    $ php symfony doctrine:reload-data

equivale ad eseguire questi comandi:

    $ php symfony doctrine:drop-db
    $ php symfony doctrine:build-db
    $ php symfony doctrine:insert-sql
    $ php symfony doctrine:data-load

#### Altri Build

Il nuovo task `doctrine:build` consente di specificare esattamente cosa si
vuole che symfony e Doctrine costruiscano. Questo task replica la
funzionalità di molte combinazioni esistenti di task, che sono state tutte
deprecate in favore di questa soluzione più flessibile.

Ecco alcuni possibili utilizzi di `doctrine:build`:

    $ php symfony doctrine:build --db --and-load

Questo elimina (`:drop-db`) e crea (`:build-db`) il database, crea le
tabelle configurate in `schema.yml` (`:insert-sql`) e carica le fixture
(`:data-load`).

    $ php symfony doctrine:build --all-classes --and-migrate

Questo costruisce il modello (`:build-model`), i form (`:build-forms`), i
filtri (`:build-filters`) ed esegue le migrazioni pendenti (`:migrate`).

    $ php symfony doctrine:build --model --and-migrate --and-append=data/fixtures/categories.yml

Questo costruisce il modello (`:build-model`), migra il database (`:migrate`)
ed appende le fixture per le categorie
(`:data-load --append --dir=data/fixtures/categories.yml`).

Per ulteriori informazioni, si veda la pagina di aiuto del task
`doctrine:build`.

#### Nuova opzione: `--migrate`

I seguenti task ora includono un'opzione `--migrate`, che sostituirà
il task annidato `doctrine:insert-sql` con `doctrine:migrate`.

  * `doctrine:build-all`
  * `doctrine:build-all-load`
  * `doctrine:build-all-reload`
  * `doctrine:build-all-reload-test-all`
  * `doctrine:rebuild-db`
  * `doctrine:reload-data`

#### `doctrine:generate-migration --editor-cmd`

Il task `doctrine:generate-migration` ora include un'opzione
`--editor-cmd`, che sarà eseguita una volta  che la classe della
migrazione sarà creata, per una facile modifica.

    $ php symfony doctrine:generate-migration AddUserEmailColumn --editor-cmd=mate

Questo esempio genererà la nuova classe di migrazione ed aprirà il nuovo
file in TextMate.

### Setter e Getter di Date

Abbiamo aggiunto nuovi metodi per recuperare i valori di date o timestamp
di Doctrine come istanze di oggetti DateTime di PHP.

    [php]
    echo $article->getDateTimeObject('created_at')
      ->format('m/d/Y');

Si posson anche impostare dei valori di date semplicemente richiamando il
metodo `setDateTimeObject` e pasando un'istanza valida `DateTime`.

    [php]
    $article->setDateTimeObject('created_at', new DateTime('09/01/1985'));

### Output dei Task DQL come Tabelle di Dati

Quando si eseguiva il comando `doctrine:dql`, questo mostrava i dati solo
come YAML. Abbiamo aggiunto una nuova opzione `--table`. Tale opzione consente
di mostrare i dati in formato tabulare, similmente a come fa MySQL da
linea di comando.

Quindi ora si può fare come segue.

    $ ./symfony doctrine:dql "FROM Article a" --table
    >> doctrine  executing dql query
    DQL: FROM Article a
    +----+-----------+----------------+---------------------+---------------------+
    | id | author_id | is_on_homepage | created_at          | updated_at          |
    +----+-----------+----------------+---------------------+---------------------+
    | 1  | 1         |                | 2009-07-07 18:02:24 | 2009-07-07 18:02:24 |
    | 2  | 2         |                | 2009-07-07 18:02:24 | 2009-07-07 18:02:24 |
    +----+-----------+----------------+---------------------+---------------------+
    (2 results)

### Debug delle query nei test funzionali

La classe `sfTesterDoctrine` ora include un metodo `->debug()`. Questo metodo
mostra informazioni sulle query che sono state eseguite nell'attuale
contesto.

    [php]
    $browser->
      get('/articles')->
      with('doctrine')->debug()
    ;

Si possono vedere solo le ultime query eseguite passando un intero al metodo
oppure mostrare solo le query che contengono una stringa o che soddisfano
un'espressione regolare, passando una stringa.

    [php]
    $browser->
      get('/articles')->
      with('doctrine')->debug('/from articles/i')
    ;

### `sfFormFilterDoctrine`

La classe `sfFormFilterDoctrine` ora può ricevere un oggetto
`Doctrine_Query` tramite l'opzione `query`:

    [php]
    $filter = new ArticleFormFilter(array(), array(
      'query' => $table->createQuery()->select('title, body'),
    ));

Il metodo specificato tramite `->setTableMethod()` (o anche tramite
l'opzione `table_method`) non è più richiesto per restituire un
oggetto query. Qualsiasi dei seguenti metodi `sfFormFilterDoctrine` è valido:

    [php]
    // works in symfony >= 1.2
    public function getQuery()
    {
      return $this->createQuery()->select('title, body');
    }

    // works in symfony >= 1.2
    public function filterQuery(Doctrine_Query $query)
    {
      return $query->select('title, body');
    }

    // works in symfony >= 1.3
    public function modifyQuery(Doctrine_Query $query)
    {
      $query->select('title, body');
    }

Web Debug Toolbar
-----------------

### `sfWebDebugPanel::setStatus()`

Ogni pannello nella web debug toolbar può specificare uno stato, che
influirà sul colore di sfondo del titolo. Ad esempio, il colore di sfondo
del pannello di log cambia quando un messaggio con priorità maggiore di
`sfLogger::INFO` viene loggato.

### Parametro di richiesta `sfWebDebugPanel`

Si può ora specificare un pannello da aprire al caricamento della pagina,
passando un parametro `sfWebDebugPanel` all'URL. Ad esempio, passando
`?sfWebDebugPanel=config`, la web debug toolbar sarà visualizzata
con il pannello di configurazione aperto.

I pannelli possono anche ispezionare i parametri di richiesta, accedendo
all'opzione `request_parameters`:

    [php]
    $requestParameters = $this->webDebug->getOption('request_parameters');

Partial
-------

### Miglioramenti agli Slot

Gli helper `get_slot()` e `include_slot()` ora accettano un secondo
parametro per specificare il contenuto predefinito dello slot, da
restituire se nessun contenuto viene fornito dallo slot stesso:

    [php]
    <?php echo get_slot('pippo', 'pluto') // mostrerà 'pluto' se lo slot 'pippo' non è definito ?>
    <?php include_slot('pippo', 'pluto') // mostrerà 'pluto' se lo slot 'pippo' non è definito ?>

Pager
-----

I metodi `sfDoctrinePager` e `sfPropelPager` ora implementano le interfacce
`Iterator` e `Countable`.

    <?php if (count($pager)): ?>
      <ul>
        <?php foreach ($pager as $article): ?>
          <li><?php echo link_to($article->getTitle(), 'article_show', $article) ?></li>
        <?php endforeach; ?>
      </ul>
    <?php else: ?>
      <p>No results.</p>
    <?php endif; ?>

Cache della Vista
-----------------

Il gestore della cache della vista ora accetta parametri in
`factories.yml`. La generazione della chiave di cache per una vista è
stata rifattorizzata in diversi metodi, per facilitare l'estensione
della classe.

Due parametri sono disponibili in `factories.yml`:

  * `cache_key_use_vary_headers` (predefinito: `true`): specifica se
    le chiavi della cache debbano includere la parte "vary" degli header.
    In pratica, dice se la cache della pagina debba essere dipendente
    dagli header http, come specificato nel parametro `vary` della cache.

  * `cache_key_use_host_name` (predefinito: `true`): specifica se
    le chiavi della cache debbano includere la parte del nome dell'host.
    In pratica, dice se la cache della pagina debba essere dipendente
    dal nome dell'host.

Richiesta
---------

### `getContent()`

Il contenuto della richiesta è ora accessibile tramite il metodo
`getContent()`.

### Parametri PUT e DELETE

Quando una richiesta arriva con metodo HTTP PUT o DELETE ed il
content-type impostato a `application/x-www-form-urlencoded`, symfony 
ora analizza il body grezzo e rende i parametri accessibili come
normali parametri POST.

Azioni
------

### `redirect()`

La famiglia dei metodi `sfAction::redirect()` è ora compatibile con la
firma di `url_for()` introdotta in in symfony 1.2:

    [php]
    // symfony 1.2
    $this->redirect(array('sf_route' => 'article_show', 'sf_subject' => $article));

    // symfony 1.3
    $this->redirect('article_show', $article);

Questo miglioramento si applica anche a `redirectIf()` e `redirectUnless()`.

Helper
------

### `link_to_if()`, `link_to_unless()`

Gli helper `link_to_if()` e `link_to_unless()` sono ora compatibili con la firma
di `link_to()` introdotta in symfony 1.2:

    [php]
    // symfony 1.2
    <?php echo link_to_unless($foo, '@article_show?id='.$article->getId()) ?>

    // symfony 1.3
    <?php echo link_to_unless($foo, 'article_show', $article) ?>
