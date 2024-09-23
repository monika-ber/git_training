## Plik .gitignore
Pliki związane z intelliJ zostały dodane do repozytorium przed powstaniem pliku .gitignore, przez co były trackowane przez gita. Żeby usunąć je z repozytorium zdalnego, ale nie usuwać ich z repozytorium lokalnego, wykorzystałam polecenie **git rm --cached** dla folderu .idea i pliku git-dtag.iml. Po wykonaniu tych operacji i umieszczeniu nazw ignorowanych plików w pliku .gitignore, pomimo, że pliki nadal istnieją lokalnie, nie będą ponownie trackowane przez gita.

## Problem napotkany podczas zmiany brancha main na test
Nie jestem w stanie zmienić gałęzi z powrotem na test, ponieważ edytowałam pliki na gałęzi main (edycja wersji w pliku pom.xml). Git nie pozwoli na przełączenie gałęzi, żeby przeprowadzone zmiany nie zostały utracone. Żeby móc ponownie zmienić gałąź można skorzystać z poniższych sposobów:
* wykonanie polecenia **git commit**, aby zatwierdzić wykonane zmiany. Można je wykorzystać, jeżeli zakończyliśmy pracę nad modyfikowanymi plikami. Nie powinniśmy commitować niedokończonych zmian, więc jeżeli nie dokończyliśmy pracy na danym branchu, ta metoda nie jest odpowiednia.
* wykonanie polecenia **git stash**, które umożliwia tymczasowe przechowywanie zmodyfikowanych plików, dzięki czemu możliwa jest zmiana brancha bez ryzyka utraty dotychczasowych zmian i commitowania niegotowych plików. Za pomocą tego polecenia można zachować wszystkie nie commitowane zmiany (zarówno te w staging area, jak i te znajdujące się nadal w working directory).

## Różnica pomiędzy poleceniami git checkout HEAD~3 a git reset --hard HEAD~3
Polecenie **git checkout HEAD~3** pozwala się na cofnięcie się o 3 commity sprzed bieżącego na danym branchu. Jest to operacja polegająca jedynie na przeniesieniu się do danego momentu w repozytorium, nowsze commity nie są w żaden sposób modyfikowane. Należy jednak pamiętać, że po wykonaniu tego polecenia nie znajdujemy się na żadnym branchu, w stanie tzw. "detached head". W przypadku polecenia **git reset --hard HEAD~3** zarówno staging area, jak i working directory zostaną przywrócone do stanu sprzed 3 ostatnio wykonanych commitów. Po wykonaniu tego polecenia nie jesteśmy w stanie odzyskać zmian z tych commitów, o które się cofnęliśmy, ponieważ zostają całkowicie usunięte z historii repozytorium i nie mogą być odzyskane.

## Różnice pomiędzy git revert a git reset
**Git reset hash_commit** pozwala na cofnięcie z repozytorium commitów wykonanych po wyznaczonym przez nas commicie. Git reset wpływa na historię repozytorium, ponieważ wycofane commity nie będą już w niej widoczne. Użycie samego polecenia **git reset** oznacza wykorzystanie **git reset --mixed**, czyli przywrócenie wszystkich zmian do working directory.  
**Git revert hash_commit** nie wpływa na zmiany w historii repozytorium, ponieważ powoduje utworzenie nowego commita, który cofa zmiany wykonane przez commity dodane po określonym przez nas commicie.

## Sprzątanie śmieci
### Git clean -n
Wykonanie polecenia **Git clean -n** spowodowało, że plik test został wylistowany, ponieważ nie został dodany do repozytorium, ale nie został usunięty. Wykonanie samego **git clean** spowoduje usunięcie wymienionych plików.

### Git remote prune -n
Polecenie **git remote prune** służy do usuwania gałęzi, które zostały usunięte z serwera, ale nadal istnieją w lokalnym repozytorium. Dodanie opcji -n pokaże które gałęzie zostały usunięte na serwerze i które będą usunięte z lokalnego repozytorium, ale nie wykona faktycznego usunięcia.

## Usuwanie nieużywanych branchy
### Usuwanie branchy lokalnych
Polecenie **git branch -d nazwaBrancha** lub **git branch -D nazwaBrancha** pozwala na usunięcie branchy w repozytorium lokalnym. Gałąź nie może być wycheckoutowana do edycji jeżeli chcemy ją usunąć.
Użycie opcji -d spowoduje usunięcie lokalnej gałęzi tylko wtedy, gdy została już spushowana i zmergowana z gałęzią na repozytorium zdalnym (origin/nazwaBrancha). 
Wykorzystanie opcji -D pozwala wymusić usunięcie gałęzi, nawet jeśli nie została jeszcze spushowana i zmergowana z gałęzią na repozytorium zdalnym (origin/nazwaBrancha). 

### Usuwanie branchy zdalnych
Polecenie **git push origin --delete nazwaBrancha** pozwala na usunięcie brancha w repozytorium zdalnym.

## Cherry-pick --no-commit
Wykorzystanie opcji --no-commit powoduje, że zmiany z wybranego commita zostają wprowadzone do staging area na obecnie wycheckoutowanym branchu, co daje użytkownikowi szansę na dodatkową weryfikację wprowadzanych zmian i ich dodatkową modyfikację przed zatwierdzeniem ich za pomocą commita.