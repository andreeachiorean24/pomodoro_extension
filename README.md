# pomodoro_extension

Extensie Browser Pomodoro
1. Descriere generală
Această extensie de browser implementează tehnica Pomodoro, o metodă de gestionare a timpului în sesiuni de lucru focusate, urmate de pauze scurte. Utilizatorul poate porni un cronometru Pomodoro, iar după ce timpul expiră, primește o notificare. Extensia este construită pentru Google Chrome și folosește API-uri oferite de platformă, precum alarms și storage.
2. Structura fișierelor
Proiectul este împărțit în mai multe fișiere și directoare, fiecare având un rol specific în arhitectura extensiei.
3. Explicația detaliată a fișierelor
•	📄 background.js
Acest fișier rulează în fundal și gestionează logica principală a cronometrului Pomodoro. Folosește API-ul `chrome.alarms` pentru a crea un timer care se declanșează la fiecare secundă. La fiecare declanșare, verifică dacă extensia este în stare activă (`isRunning`). Dacă este, incrementează timpul (`timer`) salvat în chrome.storage.local. Când timpul ajunge la valoarea setată (de exemplu 25 de minute), se declanșează o notificare folosind `registration.showNotification()` și se oprește automat cronometrul.
•	📄 manifest.json
Fișierul de configurare JSON necesar pentru orice extensie Chrome. Specifică versiunea manifestului (v3), numele extensiei, descrierea, permisiunile necesare (ex: alarms, storage), precum și ce fișiere sunt utilizate (ex: background script, popup, options). De asemenea, definește pictograma extensiei și acțiunea implicită a acesteia.
•	📄 icon.png
Imagine PNG folosită ca pictogramă a extensiei, afișată atât în bara de extensii a browserului, cât și în notificări. Este important ca dimensiunile să fie standard (de exemplu 128x128) pentru compatibilitate.
•	📄 options/options.html
Pagina HTML pentru configurarea extensiei. Aici utilizatorul poate introduce un număr care definește durata unei sesiuni Pomodoro. Pagina conține un formular simplu cu input numeric și un buton de salvare.
•	📄 options/options.css
Fișier CSS care stilizează pagina de opțiuni. Definește culoarea de fundal (roșu închis), fonturile, dimensiunea și alinierea elementelor. Designul este simplu, dar plăcut vizual, pentru a nu distrage atenția de la funcționalitate.
•	📄 options/options.js
Scriptul JavaScript asociat paginii de opțiuni. Preia valoarea introdusă de utilizator și o salvează în `chrome.storage.local`. De asemenea, la încărcarea paginii, citește valoarea curentă salvată și o afișează în input-ul HTML pentru a fi modificată.
•	📄 popup/popup.html
Interfața principală a extensiei. Este afișată când utilizatorul dă clic pe pictograma extensiei. Conține o reprezentare grafică a timpului Pomodoro, un cronometru digital și butoane pentru start/pauză/reset. Pagina este compactă și destinată interacțiunii rapide.
•	📄 popup/popup.css
Fișierul CSS care stilizează interfața popup. Folosește culori contrastante, fonturi mari și butoane bine definite pentru a permite o utilizare rapidă și clară. Are un design centrat și responsive.
•	📄 popup/popup.js
Logica interfeței popup. La apăsarea butonului de start, trimite un semnal către `background.js` pentru a începe cronometrul. Actualizează dinamic timpul afișat și sincronizează starea cu valorile din storage. Folosește funcții asincrone și event listeners pentru a răspunde la modificările în timp real.
4. Funcționalități implementate
• Timer Pomodoro cu durată configurabilă
• Persistența stării cronometrului cu `chrome.storage`
• Interfață popup modernă și ușor de folosit
• Pagina de setări dedicată pentru utilizator
• Comunicare între componente cu API-ul `chrome.runtime`
5. Instalare și testare
1. Deschideți Google Chrome și accesați chrome://extensions/
2. Activați 'Developer Mode' (Modul dezvoltator)
3. Apăsați pe 'Load unpacked' și selectați folderul `end`
4. Extensia va apărea în bara de extensii și poate fi folosită imediat

6. Integrare într-o aplicație existentă
4.1 Publicare în Chrome Web Store
Pentru a permite utilizatorilor tăi să instaleze extensia direct din aplicație, trebuie mai întâi să o publici în Chrome Web Store:
- Accesează Chrome Web Store Developer Dashboard.
- Încarcă pachetul ZIP al folderului end/.
- Completează toate câmpurile cerute (descriere, capturi de ecran, categorii etc.).
4.2 Adăugare link în aplicația web
Poți adăuga un buton sau un link în aplicația ta pentru a direcționa utilizatorul către extensie:
<a href="https://chrome.google.com/webstore/detail/ID-EXTENSIE" target="_blank">Instalează extensia Pomodoro</a>
4.3 Comunicare între aplicație și extensie
Pentru o integrare mai profundă, se poate stabili un canal de comunicare între aplicație și extensie.

a. Din aplicație către extensie
- Adaugă în manifest.json următoarea secțiune:
{
  "externally_connectable": {
    "matches": ["https://domeniul-tau.com/*"]
  }
}

- Cod pentru ascultarea mesajelor în extensie:
chrome.runtime.onMessageExternal.addListener((request, sender, sendResponse) => {
  if (request.command === "startPomodoro") {
    startPomodoroLogic();
    sendResponse({ status: "started" });
  }
});

- Trimiterea mesajului din aplicație:
chrome.runtime.sendMessage("EXTENSION_ID", { command: "startPomodoro" }, response => {
  console.log(response.status);
});

b. Din extensie către aplicație
Poți folosi window.postMessage() sau poți trimite date către un API extern.

