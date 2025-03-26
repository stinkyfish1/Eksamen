# Flask Autentiseringssystem - Teknisk Dokumentasjon

## Oversikt
Dette Flask-applikasjonen implementerer et autentiseringssystem med innlogging, utlogging og registrering. Den bruker SQLite som database og Flask-Login for sesjonsstyring.

## Databehandling
### Database: SQLite
Applikasjonen lagrer brukerdata i en SQLite-database (`database.db`), konfigurert med SQLAlchemy.

### Brukermodell
Brukermodellen lagrer brukernavn og passord i databasen, der passordene er lagret som hashede verdier for å beskytte dataene.

```python
class User(db.Model, UserMixin):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(20), nullable=False, unique=True)
    password = db.Column(db.String(80), nullable=False)
```

### Passordbeskyttelse
For å sikre passordene brukes Flask-Bcrypt til å hashe passordene før de lagres i databasen.

```python
hashed_password = bcrypt.generate_password_hash(form.password.data)
```

Passordene lagres aldri i klartekst, noe som reduserer risikoen for datalekkasjer.

### Autentisering og Sesjonsstyring
Flask-Login håndterer autentisering og brukerens sesjon.

- Brukerens sesjon lastes basert på `user_id`.
- Innlogging skjer ved å sammenligne passordet som er oppgitt med det hashede passordet i databasen.
- Utlogging avslutter sesjonen og videresender brukeren til innloggingssiden.

### Beskyttede Ruter
Dashboardet krever at brukeren er logget inn for å få tilgang, og bruker `@login_required` for å beskytte ruten.

## Konklusjon
Dette systemet beskytter brukernes data ved å bruke sikker passordlagring og sesjonsstyring. Bruk av hashede passord og Flask-Login sikrer at bare autoriserte brukere får tilgang til beskyttede ressurser.