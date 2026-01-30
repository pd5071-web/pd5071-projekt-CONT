# Mikroserwis do analizy plików FASTQ za pomocą FastQC i wyświetlania wyników przez NGINX.

## Opis
Projekt przedstawia mikroserwis do analizy plików FASTQ z wykorzystaniem narzędzia FastQC.
Wyniki analizy są zapisywane w wolumenie Docker i udostępniane przez serwer WWW NGINX.
Całość projektu jest uruchamiana za pomocą Docker Compose.

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
mkdir pd5071-projekt-CONT  ~  Tworzy główny katalog projektu.
cd pd5071-projekt-CONT  ~  Przechodzi do katalogu projektu.
mkdir input  ~  Tworzy katalog na pliki FASTQ analizowane przez FastQC.

Do katalogu input skopiowano plik:
SRR8786200_1.fastq.gz z Pobranych plików
ls -lh input  ~  Sprawdza obecność i rozmiar pliku FASTQ.

### 2. Budowa obrazu FastQC
Obraz FastQC jest budowany na podstawie pliku Dockerfile, który instaluje środowisko Java oraz narzędzie FastQC oraz definiuje polecenie startowe kontenera.
nano Dockerfile  ~  Tworzy plik Dockerfile definiujący obraz FastQC.

### 3. Konfiguracja NGINX
Plik nginx.conf zawiera konfigurację serwera NGINX (port 80), który udostępnia raport HTML wygenerowany przez FastQC.
nano nginx.conf  ~  Tworzy plik konfiguracyjny NGINX.

### 4. Konfiguracja Docker Compose
Plik docker-compose.yml buduje obraz FastQC z Dockerfile, uruchamia kontener NGINX z obrazu nginx:latest, tworzy dedykowaną sieć Docker, tworzy wolumen do przechowywania wyników, mapuje port 8080 hosta na port 80 kontenera NGINX.
nano docker-compose.yml  ~  Tworzy plik uruchamiający projekt.
W tresci instrukcji zmieniono ścieżkę z powodu niepoprawnego odczytania, wyskakującego ostrzeżenia, z:
command: /input/SRR8786200_1.fastq.gz -o /results ;
na:
command: fastqc /input/SRR8786200_1.fastq.gz --outdir /results

### 5. Uruchomienie projektu
Projekt uruchamiany jest za pomocą polecenia:
docker compose up --build  ~  Komenda  uruchamiania kontener FastQC, który wykonuje analizę pliku FASTQ (wyniki zapisane w wolumenie), i kontener NGINX, który udostępnia wyniki analizy.

### 6. Sprawdzenie wolumenu z wynikami
docker volume ls  ~  Wyświetla listę wolumenów Docker.
docker exec -it nginx_container ls /usr/share/nginx/html  ~  Sprawdza, czy raport FastQC został wygenerowany i zapisany w wolumenie.

Raport FastQC jest dostępny w przeglądarce pod adresem:
http://localhost:8080

### Wolumeny i sieć
- fastqc_results – wolumen przechowujący wyniki analizy
- fastqc_network – sieć Docker do komunikacji kontenerów

### 7. Zatrzymanie projektu
docker compose down  ~  Zatrzymuje i usuwa kontenery projektu.
