# Ohjelmiston asennusohje

### Esitoimet ja oletukset

* Ohjelmaa käyttävällä tietokoneella tulee olla asennettuna Python 3.
Ubuntussa Python 3:n asentaminen onnistuu pääkäyttäjäoikeuksilla komennolla:
```
sudo apt-get install python3
```
* Verkkoon (Heroku) siirrettäessä, käyttäjällä on a) Herokun käyttäjätunnus b) Asennettuna [Herokun komentorivityökalut](https://devcenter.heroku.com/articles/heroku-cli "Heroku CLI")

### Paikallisesti toimivan ohjelmiston asennus

* Lataa ohjelman viimeisin versio zip-tiedostona Githubista kohdasta [Download](https://github.com/sokkanen/TSOHA_OL_Booking) ja pura zip-tiedosto haluamaasi kansioon.
* Mene komentorivillä ohjelman kansioon, ja luo ohjelmalle virtuaaliympäristö komennolla "python3 -m venv venv"
* Aktivoi virtuaaliympäristö komennolla "source venv/bin/activate" 
* Lataa ohjelman tarvitsemat riippuvuudet komennolla "pip install -r requirements.txt"
* Suorita ohjelma komennolla "python3 run.py"

Ohjelma toimii localhostin portissa 5000. (Selaimella: http://localhost:5000)

### Herokussa toimiva ohjelmisto

* Toimi kuten paikallisesti toimivan ohjelmiston kanssa, mutta älä käynnistä ohjelmaa.
* Varmista, ettei requirements.txt -tiedostossa ole listattuna "pkg-resources==0.0.0" -riviä. Jos on, poista.
* Lisätään Procfile -tiedosto, jonka perusteella Heroku käynnistää ohjelman. Procfilen lisääminen onnistuu Linux:n terminaalissa seuraavalla komennolla:
```
echo "web: gunicorn --preload --workers 1 hello:app" > Procfile
```
* Luodaan Herokuun sovellukselle paikka komennolla. Kirjoita haluamasi nimi "projektin_nimi" paikalle:
```
heroku create projektin_nimi
```
* Luo ohjelmalle versionhallinta komennolla:
```
git init
```
* Muodostetaan juuri luodusta Herokun osoitteesta etärepositorio komennolla:
```
git remote add heroku https://git.heroku.com/projektin_nimi.git
```
* Lähetetään ohjelma Herokuun:
```
git add .
```
```
git commit -m"kommentti"
```
```
git push heroku master
```
* Luodaan ohjelmalle ympäristömuuttuja Herokuun:
```
heroku config:set HEROKU=1
```
* Luodaan ohjelmalle tietokanta Herokuun:
```
heroku addons:add heroku-postgresql:hobby-dev
```
* Luodaan ohjelmalle käyttäjä, jotta ohjelman käyttöönotto onnistuu:
```
heroku pg:psql
```
```
INSERT INTO account (username, password, role) VALUES ('testi', '$2b$12$J0lGxspdYNnc9XxqmijUs.z0mGMyZauQoMqATVsAre6AjCCVWy45G', 'ADMIN')
```
```
\q
```
Nyt ohjelmaan on mahdollista kirjautua Herokussa osoitteessa: https://projektin_nimi.herokuapp.com/ käyttäjätunnuksella "testi" ja salasanalla "testi"
