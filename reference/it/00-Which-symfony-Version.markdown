Quale versione di symfony?
==========================

Questo libro è stato scritto sia per symfony 1.3 che per symfony 1.4.
Essendo che scrivere un unico libro per due diverse versioni di un software
è abbastanza inusuale, questo breve capitolo spiega come sia possibile,
quali sono le principali differenze tra le due versioni e come fare la scelta
migliore per i propri progetti.

Entrambe le versioni 1.3 e 1.4 di symfony sono state rilasciate all'incirca
nello stesso momento (alla fine del 2009). È un dato di fatto, entrambe hanno
**lo stesso insieme di funzionalità**. L'unica differenza tra le due versioni è
su come supportano la compatibilità all'indietro con le versioni precedenti di
symfony.

Symfony 1.3 è la versione da utilizzare quando si ha bisogno di aggiornare un
progetto che usa una delle versioni precedenti (1.0, 1.1, o 1.2) di symfony.
Ha un livello di compatibilità all'indietro e ha anche disponibili tutte
le caratteristiche che sono state deprecate durante il ciclo di sviluppo della 1.3.
Ciò significa che l'aggiornamento è facile, semplice e sicuro.

Ma se si inizia oggi un nuovo progetto, si dovrebbe usare symfony 1.4. Questa versione
ha le stesse funzionalità di symfony 1.3 ma tutte le caratteristiche deprecate e
lo strato di compatibilità sono stati rimossi. Questa versione è più pulita
e anche leggermente più veloce di symfony 1.3. Un altro grande vantaggio
nell'utilizzare symfony 1.4 è il suo lungo supporto. Essendo una versione con
supporto a lungo termine, è mantenuta dal team di symfony per tre anni (fino a
Novembre 2012).

Naturalmente, è possibile migrare i propri progetti a symfony 1.3 e progressivamente
aggiornare il codice per rimuovere le caratteristiche deprecate ed eventualmente
passare a symfony 1.4 per beneficiare del supporto a lungo termine. Si ha tutto il
tempo per pianificare la migrazione, dal momento che symfony 1.3 è supportato per
un anno (fino a novembre 2010).

Essendo che questo libro non utilizza caratteristiche deprecate, tutti gli esempi
funzionano altrettanto bene su entrambe le versioni.
