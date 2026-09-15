# Daily Email — trimitere automată de mesaje pe email

## Cum funcționează
- `message.md` = fișierul pe care îl editezi tu ori de câte ori vrei să schimbi ce se trimite.
- `.github/workflows/daily-email.yml` = workflow-ul GitHub Actions care rulează zilnic (cron) și trimite conținutul din `message.md` pe email.

## Pași de configurare

### 1. Creează un repo pe GitHub
Pune aceste fișiere (`README.md`, `message.md`, `.github/workflows/daily-email.yml`) într-un repo nou (poate fi și privat).

### 2. Obține o "App Password" de Gmail (dacă folosești Gmail)
Gmail nu permite folosirea parolei normale pentru SMTP din aplicații terțe. Trebuie să generezi o parolă de aplicație:
1. Activează verificarea în 2 pași pe contul Google: https://myaccount.google.com/security
2. Mergi la https://myaccount.google.com/apppasswords
3. Generează o parolă nouă (ex: "GitHub Actions") — vei primi un cod de 16 caractere.

> Poți folosi și alt furnizor SMTP (Outlook, Yahoo, propriul server) — doar schimbi `server_address` / `server_port` din workflow.

### 3. Adaugă secretele în repo
În repo → **Settings** → **Secrets and variables** → **Actions** → **New repository secret**, adaugă:

| Nume secret       | Valoare                                  |
|--------------------|-------------------------------------------|
| `MAIL_USERNAME`    | adresa ta de gmail (ex: `eu@gmail.com`)   |
| `MAIL_PASSWORD`    | parola de aplicație generată la pasul 2   |
| `MAIL_TO`          | adresa unde vrei să primești mailul       |

### 4. Ajustează ora de trimitere
În `daily-email.yml`, linia:
```yaml
- cron: "0 7 * * *"
```
Ora este în **UTC**. Exemple:
- `0 6 * * *` → 06:00 UTC = 08:00/09:00 România
- `30 18 * * *` → 18:30 UTC = 20:30/21:30 România

Poți genera ușor expresia cron pe https://crontab.guru/

### 5. Testează manual
După ce faci push, mergi în tab-ul **Actions** al repo-ului → selectează workflow-ul "Daily Email" → **Run workflow**, ca să testezi imediat fără să aștepți programarea.

## Cum trimiți alt mesaj în fiecare zi
Pur și simplu editezi `message.md` (direct pe GitHub, din browser, e suficient) și salvezi (commit). La următoarea rulare programată, mailul va conține noul text din fișier.

## Note
- GitHub Actions poate întârzia rularea cron cu câteva minute în perioadele aglomerate — e normal, nu e un bug.
- Dacă repo-ul e privat și nu ai activitate o vreme, GitHub Actions dezactivează automat workflow-urile programate după **60 de zile** fără commit-uri. Soluție: fă un commit ocazional (chiar și pe `message.md`) sau reactivează manual din tab-ul Actions.
