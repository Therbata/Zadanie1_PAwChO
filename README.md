# Zadanie 1 – Programowanie Aplikacji w Chmurze Obliczeniowej

Autor: **Jakub Kramek**  
Port: **8080**  
Data: **2025-05-20**

## Opis

Aplikacja webowa napisana w Pythonie z użyciem Flask. Po uruchomieniu w kontenerze Docker:
- loguje datę uruchomienia, imię i nazwisko autora oraz numer portu,
- udostępnia interfejs HTML umożliwiający wybór miasta i wyświetla aktualną pogodę (temperatura, wiatr) na podstawie danych z API Open-Meteo.

## Uruchomienie

### Budowanie obrazu

```bash
docker build -t jakubkramek/zadanie1-flask .
```

### Uruchamianie kontenera

```bash
docker run -d -p 8080:8080 --name flask-pogoda jakubkramek/zadanie1-flask
```

### Podgląd działania

W przeglądarce: [http://localhost:8080](http://localhost:8080)

### Logi aplikacji

```bash
docker logs flask-pogoda
```

### Sprawdzenie warstw i rozmiaru

```bash
docker image inspect jakubkramek/zadanie1-flask --format='{.RootFS.Layers}'
docker images jakubkramek/zadanie1-flask
```

## Pliki w repozytorium

- `app.py` – kod źródłowy aplikacji
- `Dockerfile` – plik do budowy obrazu
- `README.md` – ten plik
