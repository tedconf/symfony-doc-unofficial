Instalacja symfony
==================

### Katalog projektu

Przed instalacją symfony, na początku musisz stworzyć katalog, w którym 
będziesz trzymać wszystkie plik związane z Twoim projektem: 

    $ mkdir -p /home/sfproject
    $ cd /home/sfproject

Lub w Windows-ie:

    c:\> mkdir c:\dev\sfproject
    c:\> cd c:\dev\sfproject

>**NOTE**
>Użytkownicy Windowsa powinni uruchamiać symfony oraz swój nowy projekt
>w ścieżce, nie zawierającej żadnych spacji w nazwie. 
>W szczególności katalogu `Documents and Settings`, oraz tym samym w  
>`Moje dokumenty`.

-

>**TIP**
>Jeżeli utworzysz projekt symfony w katalogu domowym serwera stron, 
>nie będziesz musiał wprowadzać żadnych zmian na serwerze stron WWW. 
>Oczywiście na serwer produkcyjny, bardzo zalecamy skonfigurować serwer
>stron wg instrukcji opisanej w części dot. konfiguracji serwera stron.  

### Instalacja symfony

Utwórz katalog do w którym będziesz trzymać pliki biblioteki frameworka symfony:

    $ mkdir -p lib/vendor

Następnie, potrzebujesz zainstalować symfony. Od kiedy framework ma kilka stabilnych
wydań, musisz przejrzeć i wybrać, którą wersję chcesz zainstalować i przeczytać
[stronę dot. instalacji](http://www.symfony.pl/instalacja/ lub http://www.symfony-project.org/installation) 
na stronie symfony.

Przejdź do strony instalacji dla wersji, jaka została wybrana, na przykład
[symfony 1.3](http://www.symfony-project.org/installation/1_3).

Pod linkiem "**Źródło**" lub "**Source Download**", znajdziesz pliki archiwum `.tgz`
lub `.zip`. Pobierz plik archiwum i umieść go w nowo utworzonym katalogu
`lib/vendor/`, a następnie rozpakuj go:

    $ cd lib/vendor
    $ tar zxpf symfony-1.3.0.tgz
    $ mv symfony-1.3.0 symfony
    $ rm symfony-1.3.0.tgz

W Windowsie, możesz rozpakować plik zip, korzystając Explorera Windows.
Po zmianie nazwy katalogu na `symfony`, struktura katalogów powinna być
podobna do `c:\dev\sfproject\lib\vendor\symfony`.

>**TIP**
>Jeżeli korzystasz z Subversion, o wiele lepiej użyć atrybutu `svn:externals`
>dołączająć go w katalogu w Twoim projekcie `lib/vendor/`, co zapewni, że 
>każdy naprawiony błąd w wersji stabilnej, zostanie automatycznie naprawiony: 
>
>     http://svn.symfony-project.com/branches/1.3/

Sprawdź czy symfony jest poprawnie zainstalowany, wywołując w wierszu poleceń 
wyświetlenie wersji symfony (zwróć uwagę na dużą literę `V`):

    $ cd ../..
    $ php lib/vendor/symfony/data/bin/symfony -V

Lub w Windowsie:

    c:\> cd ..\..
    c:\> php lib\vendor\symfony\data\bin\symfony -V

>**TIP**
>Jeżeli jesteś ciekawy na temat dostępnych komend w wierszu poleceń, napisz
>`symfony` aby wyświetlić listę dostępnych opcji i zadań:
>
>     $ php lib/vendor/symfony/data/bin/symfony
>
>W Windowsie:
>
>     c:\> php lib\vendor\symfony\data\bin\symfony
>
>Wiersz poleceń symfony jest dla programisty programisty najlepszym przyjacielem. 
>Wprowadza on wiele dodatków które usprawnią twoją codzienną pracę, np.  
>wyczyści pamięć cache, generuje kod i wiele więcej.
