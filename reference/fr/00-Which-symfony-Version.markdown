Quelle version symfony ?
======================

Ce livre a �t� �crit pour les deux versions (symfony 1.3 et symfony 1.4). Bien que l'�criture
d'un livre unique pour deux versions diff�rentes d'un logiciel est assez inhabituel, ce
court chapitre explique comment c'est possible, quelles sont les principales diff�rences
entre les deux versions, et comment faire le meilleur choix pour vos projets.

Symfony 1.3 et symfony 1.4 ont �t� livr�es � peu pr�s au m�me
moment (� la fin de 2009). En fait, ils ont tous les deux **exactement
les m�mes fonctionnalit�s**. La seule diff�rence entre les deux versions est sur la mani�re dont ils
soutiennent la compatibilit� descendante avec les anciennes versions de symfony.

Symfony 1.3 est la version � utiliser si vous devez mettre � jour un
projet qui utilise une ancienne version de symfony (1.0, 1.1 ou 1.2). Elle a une
couche de compatibilit� ascendante et toutes les fonctionnalit�s qui ont �t� d�pr�ci�es
au cours du cycle de vie du d�veloppement 1.3 sont encore disponibles. Cela signifie que
l'�volution est facile, simple et s�r.

Mais si vous d�marrez un nouveau projet aujourd'hui, vous devez utiliser symfony 1.4. Cette version
poss�de les m�mes fonctionnalit�s que symfony 1.3, mais toutes les fonctionnalit�s d�pr�ci�es et
la couche de compatibilit� a �t� supprim�e. Cette version est plus propre et aussi un peu plus
rapide que symfony 1.3. Un autre grand avantage de l'utilisation de symfony 1.4 est son
plus long support. Avoir une version support�e � long terme, c'est un maintien par l'�quipe
de symfony pendant trois ans (jusqu'en Novembre 2012).

Bien s�r, vous pouvez migrer vos projets vers symfony 1.3 et puis, lentement, mettre � jour
votre code pour supprimer les fonctionnalit�s obsol�tes et finir par passer � symfony 1.4
pour b�n�ficier du support � long terme. Vous avez beaucoup plus de temps pour planifier
le changement de symfony 1.3, qui est pris en charge pour une ann�e (jusqu'en Novembre 2010).

Comme ce livre ne d�crit pas les fonctionnalit�s d�pr�ci�es, tous les exemples fonctionnent aussi
bien sur les deux versions.