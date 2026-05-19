---
title: "HomeLab #001 - Dlaczego Dell OptiPlex to świetny sprzęt do klastra Proxmox"
date: 2026-05-14
draft: false
description: Dlaczego małe biznesowe Delle są idealne do nauki Proxmox, homelabu i psucia wirtualnych maszyn zamiast prawdziwego serwera.
tags:
  - Proxmox
  - Homelab
  - DellOptiPlex
  - Virtualization
categories:
  - Homelab
author: Jan Wróbel
toc: true
cover:
  image: images/proxmox/3dells3080.jpg
  alt: Dell Optiplex 3080 Proxmox Cluster
  caption: Małe Delle, wielki chaos
---
## Mój setup

Mój aktualny klaster Proxmox będzie się składał z następującego gruzu:

- 3 × Dell OptiPlex 3080 Micro z Intel i5-10500
- 1 × Dell OptiPlex 3070 SFF z Intel i5-9500

Czy to jest enterprise data center?

![Yes](/images/proxmox/enterprise-yes.jpg)

Czy wygląda jak pudełka po ptasim mleczku?

Tak.

Czy można się na tym realnie nauczyć Proxmoxa, klastrów, VM-ek, backupów, migracji, storage, monitoringu i całej tej magii?

Zdecydowanie tak.

## Dlaczego akurat Dell OptiPlex?

Ponieważ jest to sprzęt, który miał pracować godzinami w biurze, a teraz pracuje w mojej szafce pod TV. Nie jest to świecący sprzęt dla graczy z wiatrakami, robiącymi szum. Delle z odpowiednio dobranymi dyskami SSD i NVMe są praktycznie bezgłośne. Jedyny dźwięk, jaki usłyszysz, to wiatraczek przypominający co jakiś czas, że jeszcze żyje.

I właśnie dlatego świetnie nadaje się do homelabu.

Firmy często wymieniają takie komputery po kilku latach, więc na rynku używanym można znaleźć naprawdę dobre sztuki w rozsądnej cenie.

![Recycle](/images/proxmox/Swanson-pc.gif)

Dla homelabu to złoto.

## Małe rozmiary mają znaczenie

Dell OptiPlex Micro jest mały.

Mały, ale wariat.

Trzy takie komputery zajmują mniej miejsca niż jeden duży tower PC. Można je postawić na półce, pod biurkiem, obok routera albo gdziekolwiek, gdzie żona jeszcze nie zauważy, że „to znowu jakiś dekoder” Polsatu.

Do domu to ma ogromne znaczenie.

Bo prawdziwy rack server brzmi fajnie tylko do momentu, kiedy:
- zaczyna wyć jak odkurzacz przemysłowy,
- grzeje pokój jak farelka,
- rachunek za prąd zaczyna wyglądać jak rata kredytu,
- a rodzina pyta, czemu w domu słychać lotnisko.

OptiPlexy są ciche, małe i dużo bardziej przyjazne do normalnego mieszkania.

## Wystarczająco mocne do nauki

i5-10500 to bardzo fajny procesor do Proxmoxa.

Ma 6 rdzeni i 12 wątków, więc spokojnie wystarczy do wielu VM-ek i kontenerów.

Na takim sprzęcie można postawić:

- Windows Server,
- Linux VM,
- Docker hosty,
- Kubernetes/k3s,
- Home Assistant,
- Pi-hole albo AdGuard,
- monitoring,
- testowe środowiska,
- własne usługi webowe,
- małe laby pod DevOps.

Czy odpalisz na tym 40 ciężkich maszyn produkcyjnych?

Nie.

Ale do nauki?

Idealnie.

To jest właśnie różnica między homelabem a udawaniem, że w domu budujemy Google Cloud.

## Klaster uczy więcej niż jeden serwer

Jeden serwer jest fajny.

Ale klaster uczy zupełnie innych rzeczy.

Dopiero przy kilku node’ach zaczynasz rozumieć:

- czym jest quorum,
- dlaczego backupy są ważne,
- jak działa migracja VM między node’ami,
- co się dzieje, gdy jeden node padnie,
- dlaczego storage potrafi zepsuć humor,
- czemu sieć jest ważniejsza niż się wydaje,
- dlaczego „działało wczoraj” nie jest dokumentacją.

I to jest prawdziwa nauka.

Nie teoria z YouTube.
Nie tutorial z idealnym labem.
Tylko realne:

> „Czemu ten node nie widzi storage?”
>
> „Czemu VM nie chce się migrować?”
>
> „Czemu klaster krzyczy o quorum?”
>
> „Czemu ja to w ogóle ruszałem?”

Czyli normalny dzień w IT, ale w domu. Jakby w pracy było tego za mało.

## Psuj VM-ki, nie jeden serwer

Największa zaleta Proxmoxa?

Możesz psuć rzeczy bez płaczu lub płacząc, ponieważ przecież „to powinno działać”.

Zamiast mieć jeden serwer, na którym wszystko działa i boisz się cokolwiek ruszyć, robisz VM-kę, po to, żeby nie działało już na wielu serwerach jednocześnie.

Potem snapshot.

Potem eksperyment.

Potem coś psujesz.

Potem patrzysz na ekran i mówisz:

> „Aha. Czyli tego nie powinienem był robić.”

I robisz rollback lub wciskasz strzałkę do góry w Terminalu i szukasz komendy, która kiedyś to naprawiła.

![ArrowUp](/images/proxmox/drake-arrow-up.png)

To jest piękne.

Zamiast psuć jeden prawdziwy serwer i nie wiedzieć jak go naprawić, lepiej psuć wiele wirtualnych maszyn i też nie wiedzieć jak je naprawić.

Ale przynajmniej masz snapshoty i backupy.

To jest różnica między katastrofą a nauką.

## Dlaczego to jest dobre dla kariery?

Taki homelab to nie tylko zabawa.

To jest praktyczna nauka infrastruktury.

Na takim klastrze możesz ćwiczyć rzeczy, które później pojawiają się w prawdziwej pracy:

- wirtualizacja,
- backup i restore,
- monitoring,
- Linux,
- sieci,
- DNS,
- reverse proxy,
- Docker,
- automatyzacja,
- troubleshooting,
- dokumentacja.

To nie jest „bawię się komputerami”.

To jest:

> „Buduję środowisko, psuję je, naprawiam i dokumentuję, czego się nauczyłem.”

A to już brzmi dużo fajniej.

## Co może pójść źle?

Oczywiście, że coś pójdzie źle.

To Proxmox.

To homelab.

To IT.

Możliwe problemy:

- brak quorum,
- brak prądu,
- błędnie ustawiony storage,
- VM nie startuje po migracji,
- restore backupu nie działa tak, jak sobie wyobrażałeś,
- sieć robi dziwne rzeczy,
- DNS kłamie,
- jeden node ma inne ustawienia niż reszta,
- aktualizacja coś zmienia,
- Ty zapominasz, co zmieniłeś wczoraj.

Dlatego właśnie warto mieć kilka małych maszyn.

Bo można testować, porównywać, wyłączać, przywracać i uczyć się na błędach.

Bez zabijania całego domowego środowiska.

Przynajmniej nie zawsze.

## TL;DR

Dell OptiPlex Micro i SFF to bardzo dobry wybór do domowego klastra Proxmox.

Są małe, ciche, tanie na rynku używanym i wystarczająco mocne do nauki.

Mój setup:

- 3 × Dell OptiPlex 3080 Micro i5-10500
- 1 × Dell OptiPlex 3070 SFF i5-9500

To nie jest enterprise data center.

Ale do nauki Proxmoxa, VM-ek, backupów, snapshotów, migracji i realnego troubleshootingu — jest idealne.

Bo w homelabie chodzi o jedno:

Psuj rzeczy.

Ale psuj je tak, żeby można było je potem przywrócić.
