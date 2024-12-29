# Commandes exécutées
curl http://localhost/
curl http://localhost/inexistante.html

# Logs analysés
127.0.0.1 - - [29/Dec/2024:15:45:12 +0000] "GET / HTTP/1.1" 200 345
127.0.0.1 - - [29/Dec/2024:15:45:15 +0000] "GET /inexistante.html HTTP/1.1" 404 123

# Observations
- Les requêtes réussies (200) montrent que la page d'accueil est accessible.
- Les erreurs 404 indiquent que la page inexistante.html n'existe pas.
- L'adresse IP la plus fréquente est 127.0.0.1 (locale).
