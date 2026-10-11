# AEM CV

Portfolio-/CV-sajt med inbyggd adminpanel, byggd i en enda `index.html` (ren HTML, CSS och JavaScript – inga byggsteg).

## Innehåll
- **Inloggning först:** hela sajten kräver inloggning (e-post och lösenord, samt Google, GitHub och LinkedIn via Firebase).
- **Sajt:** Hero, Erfarenhet (jobbtitel, beskrivning, start- och slutdatum med automatisk längd), Personuppgifter (förnamn, efternamn, personnummer, adress, körkort och språk med nivå 0–100), Statistik, Filosofi, Omdömen, Kontakt och Footer med sociala ikoner.
- **Adminpanel (`#/admin`):** redigera allt innehåll, tema, typsnitt, färger, bilder (med automatisk svartvit bearbetning och bakgrundsborttagning av profilbilden), Ångra/Gör om, autospar, export/import av JSON och återställning.
- Responsiv för mobil, tablet och desktop. Allt är på svenska.

## Kom igång
Öppna `index.html` via en lokal server (inte `file://`), till exempel:

```
npx serve
```

Utan Firebase-uppgifter körs sajten i **lokalt läge**: första registreringen skapar adminkontot och sparas i webbläsaren. Sociala inloggningar är då avstängda.

## Publicera på GitHub Pages
1. Repo → **Settings → Pages**.
2. *Source:* **Deploy from a branch**, branch `main`, mapp `/ (root)` → **Save**.
3. Sajten blir tillgänglig på `https://essgrombalia-png.github.io/AEM-CV/`.

## Aktivera Firebase-inloggning (Google, GitHub, LinkedIn)
1. Skapa ett projekt på https://console.firebase.google.com → **Authentication → Sign-in method** och aktivera Email/Password, Google och GitHub.
   - **GitHub:** skapa en OAuth App på GitHub och sätt *Authorization callback URL* till adressen Firebase visar (`https://DITT-PROJEKT.firebaseapp.com/__/auth/handler`).
   - **LinkedIn:** kräver uppgradering till Identity Platform och en OpenID Connect-leverantör med Provider ID `linkedin` (kontrollera i konsolen om uppgraderingen kräver betalkort).
2. **Project settings → Your apps → Web** → kopiera konfigurationen till `window.FIREBASE_CONFIG` överst i `index.html`.
3. Skriv din e-postadress i `window.ADMIN_EMAILS` (bara dessa får öppna `#/admin`).
4. **Authentication → Settings → Authorized domains:** lägg till `essgrombalia-png.github.io`.

> Firebase webb-nycklar (`apiKey` m.m.) är avsedda att ligga i klientkod och är inte hemliga – skyddet ligger i *Authorized domains*. **Lägg aldrig** in GitHubs eller LinkedIns *client secret* i repot; de skrivs bara in i Firebase-konsolen.

## Snabbmeny
Tryck **Ctrl + K** (eller klicka på *Snabbmeny* i footern) för att hoppa mellan sektioner, ladda ner PDF, växla mörkt/ljust läge och mer.

## Dela sajten (skrivskyddad)
Klicka på **Dela** i headern (eller i admin). Du kan ladda ner CV:t som **PDF**, ladda ner en skrivskyddad `index.html` att publicera (till exempel på GitHub Pages) eller kopiera en delningslänk. PDF:en och HTML-filen fungerar var som helst, utan inloggning. Den som öppnar den ser sajten utan inloggning, kan ladda ner CV:t som PDF, utan admin och utan möjlighet att ändra något. Kontaktformuläret ersätts av en e-postknapp. Personnumret delas så som det visas på sajten (som standard utan de fyra sista siffrorna).

## Att veta
- **Personnummer:** syns för alla som kan logga in på sajten. Som standard döljs de fyra sista siffrorna; lägg bara in uppgifterna du vill visa. Lägg dem inte i publik kod om du publicerar sajten öppet.
- Innehållet du redigerar i admin sparas i webbläsarens localStorage (syns bara i den webbläsaren). Använd **Verktyg → Exportera** för att flytta innehållet.
- `ADMIN_EMAILS` kontrolleras i webbläsaren och är inget serverskydd.

## Roller: admin och vanliga användare

- **Admin** är bara `essgrombalia@gmail.com` (konstanten `ADMIN_MAIL` i koden, samt `ADMIN_EMAILS` i Firebase-blocket). Med Firebase krävs dessutom verifierad e-post.
- **Alla andra konton** är vanliga användare. De ser sajten och kan öppna "Redigera CV", där de bara kan ändra text (hero, erfarenhet, personuppgifter, språk, om mig, kontakt) och profilbilden med samma verktyg som admin (uppladdning, storlek, zoom, position, form, effekter, ram, layoutstilar och cirkel). Design, tema, navigation, inloggningssida, betalning, export/import och återställning är endast för admin.
- Vanliga användares ändringar sparas separat per konto (`pf-ucfg:<e-post>`) och ändrar aldrig admins sajt.

**Viktigt om säkerhet:** all data ligger i webbläsarens localStorage, så rollerna begränsar gränssnittet men är inte skydd mot någon som öppnar utvecklarverktygen i sin egen webbläsare. För riktigt skydd behövs en server (till exempel Firestore med regler som bara låter `essgrombalia@gmail.com` skriva sajtens konfiguration och låter varje användare skriva sitt eget dokument).

## Installera som app (iPad, iPhone, Android, dator)

Appen kan installeras, men sidan måste ligga på en **https://-adress**. Den fungerar inte att installera från en lokal fil eller inuti en förhandsvisning.

1. Lägg hela mappen på GitHub Pages (Settings → Pages → Deploy from a branch → `main` / root). Du får en adress som `https://<användare>.github.io/AEM-CV/`. Filerna `index.html`, `manifest.webmanifest`, `sw.js` och mappen `icons/` ska ligga tillsammans.
2. **iPad/iPhone:** öppna adressen i Safari → tryck på Dela-ikonen → *Lägg till på hemskärmen* → *Lägg till*.
3. **Android/Chrome/Edge:** tryck på *Installera appen* i sidan (eller webbläsarens meny → *Installera app*).

Den installerade appen har egen lagring och egen session. Du loggar alltså in igen när du öppnar den, och data du redigerat i Safari följer inte med.
