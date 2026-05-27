# MailFlow – Klient pocztowy

## Wymagania (jednorazowa instalacja)

1. **Node.js 20+** — pobierz z: https://nodejs.org (wybierz "LTS")
2. **Git** (opcjonalnie) — https://git-scm.com

## Pierwsze uruchomienie

1. Otwórz folder `mailflow` w Eksploratorze plików
2. Kliknij prawym przyciskiem myszy → "Otwórz terminal" (lub PowerShell)
3. Wpisz:

```
npm install
npm start
```

Pierwsze `npm install` zajmie kilka minut (pobiera zależności). Potem aplikacja startuje w kilka sekund.

## Budowanie instalatora .exe (jednorazowo)

```
npm run build
```

Instalator znajdziesz w folderze `dist/`. Gotowy plik `.exe` można zainstalować na dowolnym komputerze Windows — bez Node.js, jak normalna aplikacja.

## Konfiguracja synchronizacji (oznaczenia między komputerami)

1. Wejdź na https://supabase.com i utwórz darmowe konto
2. Utwórz nowy projekt
3. Wejdź w Settings → API → skopiuj "Project URL" i "anon public" key
4. W MailFlow: Ustawienia → wklej oba → Zapisz

**Ważne:** Ten sam klucz Supabase na wszystkich komputerach = wspólne oznaczenia i notatki.

W panelu Supabase utwórz tabelę `annotations`:
```sql
create table annotations (
  id uuid default gen_random_uuid() primary key,
  message_id text unique not null,
  color text,
  starred int default 0,
  note text,
  note_author text,
  updated_at bigint,
  created_at timestamptz default now()
);
alter table annotations enable row level security;
create policy "public" on annotations for all using (true) with check (true);
```

## Tłumaczenie AI

1. Wejdź na https://console.anthropic.com
2. API Keys → Create Key
3. W MailFlow: Ustawienia → wklej klucz → Zapisz

## Skróty klawiaturowe

| Skrót | Akcja |
|-------|-------|
| Ctrl+N | Nowa wiadomość |
| Ctrl+T | Szablony |
| Ctrl+1..9 | Ulubiony szablon |
| Ctrl+Enter | Wyślij (w oknie redaktora) |
| Escape | Zamknij okno/modal |

## Gmail – uwaga

Dla kont Gmail potrzebne jest **Hasło aplikacji** (nie zwykłe hasło):
1. Konto Google → Bezpieczeństwo → Weryfikacja dwuetapowa (musi być włączona)
2. Hasła aplikacji → Utwórz → "Poczta" → kopiuj 16-znakowe hasło
3. Użyj tego hasła w kreatorze konta MailFlow

## Lokalizacja danych

Wszystkie dane lokalne (SQLite) są w:
`C:\Users\[Twoja nazwa]\AppData\Roaming\mailflow\`

Plik `mailflow.db` zawiera: kontakty, szablony, todo, reguły, adnotacje.
