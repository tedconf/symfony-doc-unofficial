I principi dei file di configurazione
=====================================

I file di configurazione di symfony si basano su un insieme di principi comuni 
e condividono alcune proprietà. Questa sezione descrive questi principi in dettaglio
e si propone come riferimento per le altre sezioni che descrivono i file di
configurazione YAML.

Cache
-----

Tutti i file di configurazione di symfony sono salvati nella cache come file
PHP da alcune classi handler dedicate a ciò. Quando l'impostazione `is_debug` è
impostata a `false` (per esempio per l'ambiente `prod`), i file YAML vengono letti
solo per la prima richiesta; la cache PHP viene invece utilizzata per tutte
le richieste successive. Questo significa che il lavoro "duro" viene fatto una
sola volta, quando il file YAML viene processato ed interpretato per la prima
volta.

>**TIP**
>Nell'ambiente `dev`, dove `is_debug` è impostato a `true` di default, 
>i file di configurazione vengono processati ogni volta che si registrano
>delle modifiche (symfony verifica la data dell'ultima modifica).

Il parsing e l'inserimento in cache di ogni file di configurazione viene eseguito
da classi handler specializzate configurate in 
[`config_handler.yml`](#chapter_14_config_handlers_yml).

Nelle sezioni seguenti quando parleremo di "compilazione" significa che la prima 
volta un file YAML viene convertito in un file PHP e memorizzato nella cache.

>**TIP**
>Per fare in modo che la cache dei file di configurazione venga ricaricata
>è possibile usare il task `cache:clear`:
>
>     $ php symfony cache:clear --type=config

Costanti
--------

*File di configurazione*: `core_compile.yml`, `factories.yml`, `generator.yml`,
`databases.yml`, `filters.yml`, `view.yml`, `autoload.yml`

Alcuni file di configurazione permettono di utilizzare costanti predefinite. Le
costanti sono dichiarate tramite appositi segnaposto usando la notazione `%XXX%`
(dove XXX è una chiave maiuscola) e sono rimpiazzate dal loro attuale valore
al momento della compilazione.

### Impostazioni della configurazione

Una costante può essere una qualsiasi impostazione definita nel file di 
configurazione `settings.yml`. La chiave segnaposto è quindi la versione maiuscola
dell'impostazione con prefisso `SF_`:

    [yml]
    logging: %SF_LOGGING_ENABLED%

Quando symfony compila i file di configurazione si occupa di sostituire tutte le
occorrenze dei segnaposto `%SF_XXX%` con i corrispondenti valori contenuti in 
`settings.yml`. Nell'esempio qui sotto sostituirà il segnaposto `SF_LOGGING_ENABLED`
con il valore dell'impostazione `logging_enabled` definita il `settings.yml`.

### Impostazioni dell'applicazione

Si possono utilizzare anche le impostazioni definite nel file di configurazione 
`app.yml` utilizzando il prefisso `APP_`.

### Costanti speciali

Di default symfony definisce quattro costanti in relazione al front controller 
corrente:

 | Costanti               | Descrizione                           | Metodo di configurazione |
 | ---------------------- | ------------------------------------- | ------------------------ |
 | ~`SF_APP`~             | Il nome dell'applicazione corrente    | `getApplication()`       |
 | ~`SF_ENVIRONMENT`~     | Il nome dell'ambiente corrente        | `getEnvironment()`       |
 | ~`SF_DEBUG`~           | Indica se il debug è attivo o meno    | `isDebug()`              |
 | ~`SF_SYMFONY_LIB_DIR`~ | La directory delle librerie symfonyry | `getSymfonyLibDir()`     |

### Le directory

Le costanti sono molto utili quando si ha bisogno di fare riferimento a directory
o a percorsi di file senza inserirli nel codice. Symfony definisce alcune costanti
per directory comuni a livello di progetto e di applicazione.

La radice della gerarchia c'è la directory root del progetto, `SF_ROOT_DIR`.
Tutte le altre costanti derivano da questa directory root.

La struttura delle directory del progetto è definita come segue:

 | Costanti           | Valore di default    |
 | ------------------ | -------------------- |
 | ~`SF_APPS_DIR`~    | `SF_ROOT_DIR/apps`   |
 | ~`SF_CONFIG_DIR`~  | `SF_ROOT_DIR/config` |
 | ~`SF_CACHE_DIR`~   | `SF_ROOT_DIR/cache`  |
 | ~`SF_DATA_DIR`~    | `SF_ROOT_DIR/data`   |
 | ~`SF_DOC_DIR`~     | `SF_ROOT_DIR/doc`    |
 | ~`SF_LIB_DIR`~     | `SF_ROOT_DIR/lib`    |
 | ~`SF_LOG_DIR`~     | `SF_ROOT_DIR/log`    |
 | ~`SF_PLUGINS_DIR`~ | `SF_ROOT_DIR/plugins`|
 | ~`SF_TEST_DIR`~    | `SF_ROOT_DIR/test`   |
 | ~`SF_WEB_DIR`~     | `SF_ROOT_DIR/web`    |
 | ~`SF_UPLOAD_DIR`~  | `SF_WEB_DIR/uploads` |

La struttura delle directory delle applicazioni è definita nella direcotry
`SF_APPS_DIR/APP_NAME`:

 | Costanti                | Valore di default      |
 | ----------------------- | ---------------------- |
 | ~`SF_APP_CONFIG_DIR`~   | `SF_APP_DIR/config`    |
 | ~`SF_APP_LIB_DIR`~      | `SF_APP_DIR/lib`       |
 | ~`SF_APP_MODULE_DIR`~   | `SF_APP_DIR/modules`   |
 | ~`SF_APP_TEMPLATE_DIR`~ | `SF_APP_DIR/templates` |
 | ~`SF_APP_I18N_DIR`~     | `SF_APP_DIR/i18n`      |


Infine, la directory della cache delle applicazioni è definita come segue:

 | Costanti                  | Valore di default                |
 | ------------------------- | -------------------------------- |
 | ~`SF_APP_BASE_CACHE_DIR`~ | `SF_CACHE_DIR/APP_NAME`          |
 | ~`SF_APP_CACHE_DIR`~      | `SF_CACHE_DIR/APP_NAME/ENV_NAME` |
 | ~`SF_TEMPLATE_CACHE_DIR`~ | `SF_APP_CACHE_DIR/template`      |
 | ~`SF_I18N_CACHE_DIR`~     | `SF_APP_CACHE_DIR/i18n`          |
 | ~`SF_CONFIG_CACHE_DIR`~   | `SF_APP_CACHE_DIR/config`        |
 | ~`SF_TEST_CACHE_DIR`~     | `SF_APP_CACHE_DIR/test`          |
 | ~`SF_MODULE_CACHE_DIR`~   | `SF_APP_CACHE_DIR/modules`       |

Consapevolezza dell'ambiente
----------------------------

*File di configurazione*: `settings.yml`, `factories.yml`, `databases.yml`,
`app.yml`

Alcuni file di configurazione di symfony sono ambiente dipendenti, la loro
interpretazione dipende dall'attuale ambiente in uso. Questi file hanno sezioni
diverse che definiscono come la configurazione deve variare per ogni ambiente.
Quando si crea una nuova applicazione symfony crea configurazioni per i tre
ambienti di default: `prod`, `test`, and `dev`:

    [yml]
    prod:
      # Configurazione dell'ambiente `prod`

    test:
      # Configurazione dell'ambiente `test`

    dev:
      # Configurazione dell'ambiente `dev`

    all:
      # Configurazione di default per tutti gli ambienti

Quando symfony necessita di un valore da un file di configurazione procede facendo
il merge (la fusione) della configurazione per l'ambiente corrente con la sezione
di configurazione definita in `all`. La sezione speciale `all` descrive la 
configurazione di default per tutti gli ambienti. Se la sezione di un ambiente 
specifico non è definita symfony ricade sulla configurazione `all`.

Configurazione a cascata
------------------------

*File di configurazione*: `core_compile.yml`, `autoload.yml`, `settings.yml`,
`factories.yml`, `databases.yml`, `security.yml`, `cache.yml`, `app.yml`,
`filters.yml`, `view.yml`

Alcuni file di configurazione possono essere definiti in molte sotto directory
`config/` contenute nella struttura delle directory del progetto.

Quando la configurazione viene compilata i valori da tutti i diversi file vengono
fusi assieme rispettando il seguente ordine di precedenza:

  * La configurazione del modulo (`PROJECT_ROOT_DIR/apps/APP_NAME/modules/MODULE_NAME/config/XXX.yml`)
  * La configurazione dell'applicazione (`PROJECT_ROOT_DIR/apps/APP_NAME/config/XXX.yml`)
  * La configurazione del progetto (`PROJECT_ROOT_DIR/config/XXX.yml`)
  * La configurazione definita nei plugin (`PROJECT_ROOT_DIR/plugins/*/config/XXX.yml`)
  * La configurazione definita nelle librerie di symfony (`SF_LIB_DIR/config/XXX.yml`)

Per esempio quanto definito in `settings.yml` nella directory di un'applicazione
eredita dalla configurazione impostata nella directory principale del progetto `config/`,
ed eventualmente dalla configurazione di default contenuta nel framework stesso
(`lib/config/config/settings.yml`).

>**TIP**
>Quando un file di configurazione è ambiente dipendente e può essere definito in 
>diverse directory si applica la seguente lista di priorità:
>
> 1. Modulo
> 2. Applicazione
> 3. Progetto
> 4. Ambiente specifico
> 5. Tutti gli ambienti
> 6. Default
