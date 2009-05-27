La Sandbox
===========

Se il vostro obiettivo � provare symfony per qualche ora, continuate a leggere questo
capitolo e vi mostreremo il modo pi� veloce per iniziare. Se invece intendete avviare
un progetto reale, potete tranquillamente saltare questo capitolo e
[andare](#chapter_04-Installazione-di-symfony) direttamente al prossimo.

Il modo pi� veloce per sperimentare symfony � installare la symfony sandbox. La
sandbox � un modo facilissimo per installare un progetto symfony pronto all'uso, gi�
configurato con alcuni pratici default. Questa � un ottima modalit� per sperimentare symfony 
senza doversi preoccupare delle problematiche di una installazione che rispetti le best practice
dello sviluppo web.

>**CAUTION**
>La sandbox � preconfigurata per utilizzare SQLite come database
>engine, dovete quindi verificare che il vostro PHP supporti SQLite (vedere il
>capitolo [Prerequisites](#chapter_02-Prerequisites) ). Potete anche
>leggere la sezione [Configurare il database](#chapter_05-Configurare_il_database)
>per imparare come cambiare il database utilizzato nella sandbox.

Potete effettuare il download della symfony sandbox nei formati `.tgz` o `.zip` dalla
[pagina di installazione](http://www.symfony-project.org/installation/1_2) di symfony
oppure direttamente ai seguenti URL:

    http://www.symfony-project.org/get/sf_sandbox_1_2.tgz

    http://www.symfony-project.org/get/sf_sandbox_1_2.zip

Scompattate i file da qualche parte nella vostra cartella radice del web e tutto � pronto.
Il vostro progetto symfony � ora accessibile richiedendo lo script `web/index.php`
dal browser.

>**CAUTION**
>Mantenere tutti i file di symfony nella cartella radice del web va bene per
>testare symfony in locale, ma � veramente una pessima idea su
>un server di produzione visto che rende tutti i meccanismi interni della vostra
>applicazione potenzialmente visibili agli utenti finali.

Ora potete completare la vostra installazione leggendo i capitoli
[Configurazione del web server](#chapter_06-Configurazione_del_web_server)
e [Gli ambienti](#chapter_07-Gli_ambienti).

>**NOTE**
>Dato che la sandbox � un normale progetto symfony dove sono stati eseguiti
>per voi alcuni task e modificate alcune configurazioni, � abbastanza facile
>utilizzarla come punto di partenza per un nuovo progetto.
>Ma tenete a mente che probabilmente dovrete adattare la configurazione; ad esempio
>cambiando i settaggi relativi alla sicurezza (vedere la configurazione di XSS
>e CSRF pi� avanti in questo tutorial).
