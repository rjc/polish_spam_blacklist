# Lista hostów i adresów IP wysyłających spam lub mailing bez zgody
Pliki są przygotowane do użycia z postfixem
*client_checks* - adresy IP
*sender_checks* - domeny

## Kto jest dodawany do listy?
Dodawane na liste są zarówno hosty (*serwery*) jak i klienci korzystajcy z usług firm wysyłających spam. Maile marketingowe/spam jest analizowany ręcznie, zapewnia to wysokoą skuteczność i dokładność.
Na listę **nie** dodaje tzw. "*common spam*" - czyli spamu masowego - od tego są wynalazki w stylu *spamassasin*

## Dlaczego?
Mam dość nachalnego marketingu zaczynającego się od "*Zapytanie o zgodę na przesłanie oferty...*" który omija istniejące prawo. Mam też dość nachalnych oferentów wysyłających wszelkiego rodzaju śmieci z ofertami sprzedarzy toarek czy ledów.

## Jak to działa?
*client_checks* i *sender_checks* blokują dostarczenie wiadomości na serwer pocztowy, odpowiadając wiadomością: "**BANNED**". Poczta nie jest dalej przetwarzana przez serwer.

## Jak skonfigurować?
Przykład:
```
smtpd_recipient_restrictions =
    permit_mynetworks,
    check_client_access cidr:/etc/postfix/polish_spam_blacklist/client_checks,
    check_sender_access hash:/etc/postfix/polish_spam_blacklist/sender_checks,
    ...
    ...
```

## Jak używać?
Aktualizuj przez `git pull` a następnie wygeneruj pliki bazy za pomocą `postmap` np.:
```
postmap client_checks
postmap sender_checks
/etc/init.d/postfix reload
```

## Dostałem się na listę, jak się z niej usunąć?
Z racji tego że lista jest dokładnie analizowana, pomyłki nie wchodzą w grę i nie ma możliwości usunięcia się z listy.

## Mam kilka adresów IP i domen które chciałbym dodać do listy, jak to zrobić?
Utwórz [pull request](https://github.com/kolargol/polish_spam_blacklist/pulls), dodając do niego źródła wiadomości email
