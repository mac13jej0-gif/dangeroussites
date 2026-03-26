# Menedżer Zagrożeń (Android)

Aplikacja Android skanująca zainstalowane aplikacje i wykrywająca niebezpieczne wpisy z listy w repozytorium GitHub.

## Funkcje

- Skanowanie zainstalowanych aplikacji (z pominięciem systemowych)
- Pobieranie listy niebezpiecznych pakietów (package name/app name) z `danger_apps.json` w GitHub
- Prośba o odinstalowanie wykrytych aplikacji
- Tryb darmowy: reklamy AdMob + ręczne skanowanie
- Tryb premium: brak reklam + automatyczne skanowanie co 6h (WorkManager)

## Struktura projektu

- `app/src/main/java/com/example/menedzerzagrozen/AppScanner.kt` - logika wykrywania
- `app/src/main/java/com/example/menedzerzagrozen/MainActivity.kt` - UI i główny flow
- `app/src/main/java/com/example/menedzerzagrozen/PremiumManager.kt` - zarządzanie trybem premium
- `app/src/main/java/com/example/menedzerzagrozen/PeriodicScanWorker.kt` - cykliczne skanowanie w tle
- `danger_apps.json` - przykładowa lista do umieszczenia na GitHub

## Przygotowanie repozytorium GitHub

1. Utwórz nowe repo w GitHub.
2. Skopiuj `danger_apps.json` do root i opublikuj:

```json
[
  "com.eicar.testapp",
  "Malicious App",
  "com.example.malware"
]
```

3. W `AppScanner.kt` zmodyfikuj URL:

```kotlin
private const val DANGER_LIST_URL = "https://raw.githubusercontent.com/<użytkownik>/<repo>/main/danger_apps.json"
```

## Build

W katalogu projektu:

```powershell
gradle wrapper
./gradlew clean assembleDebug
```

## Uwaga prawna

- `QUERY_ALL_PACKAGES` może być blokowane przez politykę Google Play. Tylko aplikacje systemowe/malware scanner.
- Deinstalacja wymaga potwierdzenia użytkownika.

## Dalsze usprawnienia

- Wprowadzić prawdziwy `Google Play BillingClient` w `PremiumManager.startPremiumPurchase`
- Obsługiwać listę w strukturze JSON z opisem, typem zagrożenia, datą aktualizacji
- Dodawać historię skanów i powiadomienia o usunięciach
