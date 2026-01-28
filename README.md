# Mikroserwis FastQC + NGINX

## Opis
Projekt przedstawia mikroserwis do analizy plików FASTQ z wykorzystaniem narzędzia FastQC.
Wyniki analizy są zapisywane w wolumenie Docker i udostępniane przez serwer WWW NGINX.
Całość projektu jest uruchamiana za pomocą Docker Compose.

## Wymagania
- Ubuntu (WSL)
- Docker
- Docker Compose

## Struktura projektu
pd5071-projekt-CONT/
- Dockerfile
- docker-compose.yml
- nginx.conf
- README.md
- input/
  - SRR8786200_1.fastq.gz

## Konfiguracja i uruchomienie

### 1. Utworzenie katalogu projektu
Utworzenie katalogu projektu oraz katalogu wejściowego:
mkdir pd5071-projekt-CONT
cd pd5071-projekt-CONT
mkdir input

Do katalogu input należy skopiować plik:
SRR8786200_1.fastq.gz

### 2. Budowa obrazu FastQC
Obraz FastQC jest budowany na podstawie pliku Dockerfile,
który instaluje środowisko Java oraz narzędzie FastQC.

### 3. Konfiguracja NGINX
Plik nginx.conf zawiera konfigurację serwera NGINX,
który udostępnia raport HTML wygenerowany przez FastQC.

### 4. Uruchomienie projektu
Projekt uruchamiany jest za pomocą polecenia:
docker compose up --build

Podczas uruchamiania kontener FastQC wykonuje analizę pliku FASTQ,
a kontener NGINX udostępnia wyniki analizy.

## Dostęp do wyników
Raport FastQC jest dostępny w przeglądarce pod adresem:
http://localhost:8080

## Wolumeny i sieć
- fastqc_results – wolumen przechowujący wyniki analizy
- fastqc_network – sieć Docker do komunikacji kontenerów
