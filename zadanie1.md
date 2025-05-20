Sprawozdanie z zadania 1 - Programowanie Aplikacji w Chmurze Obliczeniowej

Autor: Jakub Kramek

Część Obowiązkowa

1. Aplikacja (max. 30%)

Aplikacja została napisana w języku Python (z użyciem Flask) i realizuje wszystkie wymagane funkcjonalności.

a. Logowanie informacji przy uruchomieniu

Po uruchomieniu kontenera, aplikacja zapisuje w logach:

- Datę uruchomienia  
- Imię i nazwisko autora programu  
- Port TCP, na którym aplikacja nasłuchuje

```python
if __name__ == "__main__":
    logging.info("=== LOGI APLIKACJI ===")
    logging.info(f"Data uruchomienia: {datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S')}")
    logging.info(f"Autor: {AUTHOR}")
    logging.info(f"Port: {PORT}")
    logging.info("=============================")
    app.run(host="0.0.0.0", port=PORT)
```

b. Funkcjonalność pogodowa

Aplikacja umożliwia:

- Wybór miasta z predefiniowanej listy (Warszawa, Berlin, Londyn)
- Zatwierdzenie wyboru poprzez przycisk
- Wyświetlenie aktualnej pogody w wybranej lokalizacji

Zakres wyświetlanych informacji pogodowych:

- Temperatura (°C)
- Prędkość wiatru (km/h)

```python
cities = {
    "warszawa": (52.23, 21.01),
    "berlin": (52.52, 13.41),
    "londyn": (51.51, -0.13),
}
```

Dane pogodowe są pobierane z API Openweathermap.

2. Dockerfile (max. 50%)

Plik Dockerfile zawiera:

- Wieloetapowe budowanie z Alpine
- Instalację `curl` i `flask`, `requests`
- Healthcheck sprawdzający dostępność strony `/`

```dockerfile
FROM python:3.12-alpine

LABEL org.opencontainers.image.authors="Jakub Kramek"

WORKDIR /app

RUN apk add --no-cache curl

COPY app.py /app
COPY hc.sh /hc
RUN chmod +x /hc

RUN pip install --no-cache-dir flask requests

EXPOSE 8080

HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3     CMD ["/hc"]

CMD ["python", "app.py"]
```

3. Polecenia (max. 20%)

a. Zbudowanie obrazu kontenera:

```bash
docker build -t zadanie1:v1 .
```

b. Uruchomienie kontenera:

```bash
docker run -d -p 8080:8080 --name zadanie1-container zadanie1:v1
```

c. Uzyskanie informacji z logów:

```bash
docker logs zadanie1-container
```

output z logów:

```
=== LOGI APLIKACJI ===
Data uruchomienia: 2025-05-20 12:00:00
Autor: Jakub Kramek
Port: 8080
=============================
```

d. Sprawdzenie liczby warstw i rozmiaru obrazu:

```bash
docker images zadanie1:v1
docker history zadanie1:v1
```

Końcowy rozmiar obrazu wyniósł 110MB
