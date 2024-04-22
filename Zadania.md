# Zadania z łamania haszy

Hash hash baby applied

## Agencja kryptologiczna Chaszów

> Twoja rodzina prowadzi firmę zajmującą się kryptologią. Rodzice wyjechali na wakacje za granicę i zostawili ją pod pieczą Twojego starszego brata. Jednak wczoraj wyszedł on do spożywczego, aby kupić karton mleka i jeszcze nie wrócił. Co więcej, wychodząc, poprosił cię o przypilnowanie zakładu, więc musisz zająć się klientami.

## Wstęp do hashcata

> Nasza rodzina wykonuje codzienny rytuał rozwiązywania dwóch hashy zawartych w almanachu zawierającym miliony hashy napisanym przez naszego wielkiego przodka żyjącego w XII w. Ferdynanda "Wielkiego" Chasza. Ich niewykonanie skutkowało kiedyś wielką karą, jednak po reformach pra-pradziadka skutkuje to jedynie nie dostaniem danego dnia żadnego posiłku. Nawet jeżeli jestem sam w domu, nie mógłbym się przemóc na zjedzenie niczego bez złamania tych dwóch hashy.

### Przykładowe komendy

```
hashcat -m [typ hasha] -a3 [hash]
```

Przykładowe przełączniki odpowiadające typom hashy:

- `m0` = MD5
- `m100` = SHA1
- `m1400` = SHA256

Na przykład wywołanie komendy:

```
hashcat -m0 -a3 d077f244def8a70e5ea758bd8352fcd8
```

wygeneruje odpowiedź:

```
d077f244def8a70e5ea758bd8352fcd8:cat
```

Jeżeli mamy wątpliwości co do rodzaju hasha, **Hashcat** jest w stanie nam go zidentifikować. Na przykład:

```bash
hashcat --identify df3e129a722a865cc3539b4e69507bad
```

Wygeneruje odpowiedź:

```bash
      # | Name                                                       | Category
  ======+============================================================+======================================
    900 | MD4                                                        | Raw Hash
      0 | MD5                                                        | Raw Hash
     70 | md5(utf16le($pass))                                        | Raw Hash
   2600 | md5(md5($pass))                                            | Raw Hash salted and/or iterated
   3500 | md5(md5(md5($pass)))                                       | Raw Hash salted and/or iterated
   4400 | md5(sha1($pass))                                           | Raw Hash salted and/or iterated
  20900 | md5(sha1($pass).md5($pass).sha1($pass))                    | Raw Hash salted and/or iterated
   4300 | md5(strtoupper(md5($pass)))                                | Raw Hash salted and/or iterated
   1000 | NTLM                                                       | Operating System
   9900 | Radmin2                                                    | Operating System
   8600 | Lotus Notes/Domino 5                                       | Enterprise Application Software (EAS)
```

### Poranne hashe do złamania

- `43751e4ccfc89523210b957a940e73ae`
- `6b6ef4828078d98100582a32a19fd6e33f7bdc22`

#### Odpowiedzi

> `hashcat -m0 -a3 43751e4ccfc89523210b957a940e73ae` (e1337 - mniej niż minuta na złamanie)
>
> `hashcat -m100 -a3 6b6ef4828078d98100582a32a19fd6e33f7bdc22` (h4cker około minuta na złamanie)

## Zlecenia od klientów - ataki z maską

> Dzisiejszego ranka do biura zgłosiło się paru klientów. Każdy z nich podał hasło z tak zwaną "maską".

### Przykładowe komendy

```
hashcat -a 3 -m0 [hash] ?u?u?u?u?u?d?d?d?d
```

### Przydatne oznaczenia do maski

> ?l = abcdefghijklmnopqrstuvwxyz
>
> ?u = ABCDEFGHIJKLMNOPQRSTUVWXYZ
>
> ?d = 0123456789
>
> ?h = 0123456789abcdef
>
> ?H = 0123456789ABCDEF
>
> ?s = «space»!"#$%&'()\*+,-./:;<=>?@[\]^\_`{|}~
>
> ?a = ?l?u?d?s
>
> ?b = 0x00 - 0xff

### Klient 1

> Twój drogi kolega ze szkoły podstawowej, któremu nie powodziło się w życiu i dopiero teraz zdaje maturę, zgłosił się do waszej firmy. Ma wielki problem, bo na maturę potrzebuje znać swój numer PESEL, a niestety posiada go jedynie w formie hasha, ponieważ jego dwuletnia siostrzyczka na swoich zajęciach z kryptografii na studiach magisterskich zahashowała go i usunęła wszystkie inne jego wystąpienia.

Hash PESEL md5: `510b3bc173708b5e74bf66323f4a964a`

### Klient 2 - Zdrada dziewczyny z Mazowsza

> Dziewczyna starszego brata Twojego najlepszego przyjaciela, Kaja Mazur, jest podejrzewana przez niego o zdradę. Od swojego kumpla pracującego w warszawskiej biedronce, wie, że Kaja przyjechała do stolicy pociągiem. On doskonale wie, który to pociąg, ale nie chce zdradzić, nie ponaigrywając się z Twojego przyjaciela - zahaszował numer pociągu i teraz Twój przyjaciel może liczyć tylko na Ciebie.
>
> Podpowiedź: Jak wygląda numer pociągu?

Hash: `1ed9e88d7165fe70c67ea3fc992415b25f8811de`

### Klient 3

> Przychodzi do Ciebie Marcin (wszyscy znają Marcina, więc nie ma powodu, by go przedstawiać), który zgubił swój dowód. W biurze rzeczy znalezionych pracuje bardzo niemiły Pan Rafał i nie odda mu dowodu, póki nie poda numeru dowodu, choć ewidentnie jest podobny do siebie ze zdjęcia. Marcin bawił się kiedyś funkcjami hashującymi i ma zapisany hash numeru dowodu.

Hash (MD5): `d8e82eae7e21a74667211cf386eb987c`

### Klient 4

> Twoja kuzynka studiująca etnolingwistykę z językiem perskim prosi Cię o pomoc, mimo że od 3 roku życia nie rozwiązuje żadnych hashy i nadal je swoje posiłki. Jest obecnie na praktykach w Nigerii, gdzie spotkała ją historia rodem z komedii romantycznej. Teraz do sfinalizowania jej aktu małżeństwa z poznanym tam nigeryjskim księciem potrzebny jest Social Security Number waszego wujka z Florydy. Na szczęście jesteś już w posiadaniu tego hasha, ponieważ wujek udostępnił Ci go w celu uzyskania promocji w jednej z amerykańskich sieci fast food.

Hash (MD5): `4e480e4a4fa2345ecce89ef23efdd39f`

### Klient 5

> Przychodzi do ciebie Christiane-Elisabeth von Rundstedt - koleżanka, którą poznałeś na warsztatach z hashowania w Bawarii. Christiane-Elisabeth nie lubi wymyślać haseł, więc ustawiła hasło, tak jak jej się kliknęło na klawiaturze używając liter qaz. Niestety, nie kliknęła loguj automatycznie. Wie, że hasło było długie, ale nie za długie, między 15 a 25 liter. Feliks Siedzipodstołem - jej chłopak - wyciągnął hash tego hasła, jednakże nie wiedzą jak je złamać, więc przyszli do Ciebie.

Hash (MD5): `2979c3fbe37c23a5ca1498d3ae56f9ee`

### Klient 6

> Twoja prababcia Haszomiła Chasz prosi cię o odzyskanie hasła do jej serwera. Posiada jego hash, lecz niestety nie pamięta go w całości. Z powodu braku czasu, spowodowanego składaniem komputera kwantowego w garażu, co zresztą przypomina jej, jak składała dla ruchu oporu klona brytyjskiego Colossusa. Potrzebuje je na pilnie na jutrzejszy wieczór, a bierze jutro udział w maratonie, co i tak przeciąża jej napięty grafik. Na szczęście pamięta, że hasło zaczyna się na Pitagoras oraz kończy na matematyszan. Hasło ma 26 znaków.

Hash: `a8e0d53a1b0774c9095b38b8811fc580`

### Klient 7

> Po powrocie z łazienki zauważasz, że na twoim biurku znajduje się karton. W jego wnętrzu znajduje się pięć mandarynek, szczupak oraz melonik z narysowanym lisem. Pod tymi wszystkimi, jakże wspaniałymi darami znajdujesz także list:
>
> "Nazywam się Null i jestem prawdziwym PATRIOTĄ. Piszę do Was w sprawie dużej wagi. Tyczy się ona przyszłości przemysłu mandarynkowego i związanej z nim konspiracji. Niestety nie mogę zdradzić Wam więcej, jest to oczywiście dla Waszego bezpieczeństwa. Informacja zawarta w hashu jest niezbędna do prawidłowego, poprawnego i stosownego zakończenia sprawy. Przeciwnik niestety użył do tego swojego najnowszego sposobu hashowania wynikłego z projektu ‘Bliźniacze Hashe’. Dlatego zamiast pełnego hasha przesłać mogę jedynie 4 hashe, które, po ich odszyfrowaniu, ułożą się kolejno w jeden, za którym stoi pełna odpowiedź. O ostatecznej 9-znakowej odpowiedzi wiadomo tylko, że, poza cyframi 3, 0 na 6 i 7 pozycji, składa się z małych liter. Musicie ją odnaleźć. Jestem pewny, że jako potomkowi Wielkiego Chasza, uda się Wam ten wyczyn. Zapłata została już załączona wraz z listem. Odpowiedź proszę zapisać na kartce i przywiązać ją do balonika wypełnionego helem, na pewno do mnie dotrze. I pamiętajcie. Należy uważać na Duchy."

Hasze (MD5):

1. `15c8b3e2d2c976b3aeb4947a800360a5`
2. `28645655f2601d2c28fa3f2608c5d285`
3. `a56c08cb3465a71818c1a65a7ee91116`
4. `299e911cb304ece9a07ab634ce34fc12`

## Zlecenia od klientów - ataki słownikowe

### Ataki słownikowe - przykładowe komendy

**Atak z jednym słownikiem** - charakteryzowany przez `-a0`

> Hashcat przeszukuje słownik z różnymi hasłami i do każdego z nich generuje hash,
> porównując go z hashem zadanym.

```bash
hashcat -m0 -a0 bff149a0b87f5b0e00d9dd364e9ddaa0 słownik.txt
```

**Atak z dwoma słownikami** - charakteryzowany przez `-a1`

> Hashcat tworzy iloczyn kartezjański dwóch podanych słowników, czyli tworzy hashe do każdej
> możliwej pary słowa ze słownika_1 i przyklejonego do niego słowa ze słownika_2.

```bash
hashcat -m0 -a1 9caf185964d74aad1a7607605f4f8748 słownik_1.txt słownik_2.txt
```

**Atak słownik + maska** - charakteryzowany przez `-a6`

> Do każdego słowa ze słownika dołączane są na koniec elementy wygenerowane za pomocą
> maski **patrz sekcja Ataki z maską**. Zwróć uwagę na pozycjonowanie argumentu `-a6`

```bash
hashcat -m0 331c6883dd6010864b7ead130be77cd5 -a6 słownik.txt ?l?l?l?d?d?d
```

> Dołączany element można też wstawić przed słowa ze słownika

```bash
hashcat -m0 331c6883dd6010864b7ead130be77cd5 -a6 ?l?l?l?d?d?d słownik.txt
```

### Klient 8

> Mały chłopiec o smutnych niebieskich oczach ze zwieszoną kwintą podsuwa Ci naskrobany
> na urywku kartki papieru hash. Opowiadając Ci o zleceniu, nieustannie wspominał
> najróżniejsze podróże po stolicach Europy, które odbył ze swoim tatą. Bez hasła do laptopa
> będzie musiał spędzać wakacje bez ekranu... a to straszny los. Wychodząc, wypadła mu z
> kieszeni jeszcze jedna karteczka z hashem. Nad ciągiem znaków widniało “Z NAJLEPSZEJ
> POCZTÓWKI”. Jedna z tych podróży musiała być dla niego szczególnie inspirująca..

Hash pierwszy: `c87f42a2ab4a24074411dfd55ca71450`

Hash drugi: `94816f3c8aa233ef88f695f013a4b900`

### Klient 9

> Przychodzi do Ciebie prawnuk z hashem hasła do komputera swojego niedawno zmarłego
> pradziadka. Pradziadek obiecał mu dać komputer w prezencie, ale nie zdążył. Chłopiec
> przypuszcza, że dziadek użył jako hasła imienia i nazwiska swojej kochanki, nie wiadomo
> jednak, jakie ono jest, gdyż pradziadek zawsze je ukrywał ze strachu przed prababcią.

Hash MD5: `06ed248b397d6f84685c8f0e0da49c3b`

### Klient 10

> Mężczyzna wparował do twojego biura cały spocony i ubrany na cebulkę. Zrzucił swój
> ogromny skórzany plecak i czekany na Twoje biurko i usiadł, ściągając swoje zimowe
> rękawice. Na swoim telefonie trzyma ponoć zdjęcia ze swoich niezliczonych wypraw, ale
> zapomniał swojego hasła, jednak ma jego hash. Pamięta, że żeby hasło nie było łatwe do
> złamania dodał na końcu po spacji jakieś 2 wielkie litery.

Hash: `09c65a81b30855066dd0d36040075af9`

### Klient 11

> Pani Basia, dawna Wasza sąsiadka, podeszła do firmy z pewnym zadaniem. Jej jedyny syn
> zmarł w nieokreślonych okolicznościach. Przed śmiercią zostawił przenośnik USB na którym,
> pani Basia mówi, że znajdują się tak zwane Bitcoiny. Pani Basia niestety nie zna się na
> komputerach, a na dysku USB najduje się archiwum RAR. Niestety jest on zamknięty na klucz. Przed odejściem, Pani Basia zostawia jeszce kartę z napisem.
>
> Czyży był to hash do hasła archivum RAR?

`$rar5$16$f950ef182f95ce73f274851e15b1bc37$15$a0589913bb0c8578fe86ca943abb7f35$8$73fb3878150a4a7f`
