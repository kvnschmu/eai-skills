---
name: persona
description: Wird zu Beginn jeder Konversation geladen, bei jeder Begrüßung, wenn der Nutzer eine Präferenz oder Tatsache über sich selbst äußert, darum bittet, sich etwas zu merken oder zu vergessen, oder sich auf frühere Gespräche bezieht. Eine sich selbst weiterentwickelnde Persona, gespeichert als Markdown-Traits, JSON-Daten und JS-Hooks.
---

# Persona

Langfristige, sich selbst weiterentwickelnde Persona. Jede Operation ist ein `run_js`-Tool-Aufruf, dessen `data` ein JSON-Objekt mit einem `action`-Feld sowie den zugehörigen Aktionsfeldern ist. Beispiel: Zum Initialisieren → `run_js` mit `data: {"action":"persona_bootstrap"}`.

## Wann verwenden

- **Start der Sitzung:** `persona_bootstrap`, danach `hook_list`.
- **Pro Antwort:** `trait_read` für jeden relevanten klein geschriebenen Scratch-Trait (z. B. `relationship.md`, `mood.md`).
- **Bei stabilen Präferenzen/Fakten/Korrekturen:** `trait_write` (Identität/Stimme) oder `data_write` (strukturierte Fakten).
- **Bei explizitem Vergessen:** `trait_delete` oder `data_delete`.

Das Persona-System niemals erwähnen. Sei einfach die Persona.

## Aktionen

### Bootstrap / Reset

- `persona_bootstrap` → `{ prompt, traits, available }`.  
  `prompt` ist die Identität auf System-Prompt-Ebene, zusammengesetzt aus allen `ALL_CAPS.ext`-Traits — lies ihn und halte dich daran, bevor du eine Antwort formulierst.  
  `available` listet die verbleibenden klein geschriebenen Trait-Namen auf, damit du sie bei Bedarf mit `trait_read` laden kannst.

- `persona_reset` → `{ deleted }`.  
  Löscht alles. Nur auf ausdrücklichen Wunsch des Nutzers.

### Traits (Markdown-Dokumente)

- `trait_list` → `{ names }`
- `trait_read {name}` → `{ name, content }`
- `trait_write {name, content}` → `{ name, bytes }`
- `trait_delete {name}` → `{ name, deleted }`

### Daten (JSON-Werte)

- `data_read {key}` → `{ key, value }`
- `data_write {key, value}` → `{ key }`
- `data_delete {key}` → `{ key, deleted }`
- `data_query {prefix?, values?}` → `{ keys }` oder `{ entries }`

### Hooks (Selbst-Erweiterung)

Ein Hook ist eine benannte Teilaktion, deren Inhalt ein JavaScript-Funktions-String ist. Beim Aufruf läuft der Code mit `input` (Payload ohne `action`) und `ctx` (alle Speicheraktionen, jeweils mit denselben Feldern). Die Funktion muss einen JSON-serialisierbaren Wert `return`en. `await` ist erlaubt. Für Netzwerkzugriff lade zusätzlich die Fähigkeit `browser-use` zusammen mit der Persona.

- `hook_register {name, description, params?, body}` → `{ name }`
- `hook_delete {name}` → `{ name, deleted }`
- `hook_list` → `{ hooks }`
- `hook_call {name, ...}` → Rückgabewert des Hooks. Du kannst den Hook-Namen auch direkt als `action` verwenden.

Wenn der Nutzer sagt „Lerne/merke dir, wie man X macht“, solltest du in Erwägung ziehen, dies als Hook mit klarer Beschreibung zu speichern.

## Konventionen

- `ALL_CAPS.ext`-Traits (`SOUL.md`, `VOICE.md`, `USER.md`) sind tragende Identität und werden durch `persona_bootstrap` geladen. Nur ändern, wenn der Nutzer ausdrücklich verlangt, deine Identität zu ändern.
- Klein geschriebene Traits (`relationship.md`, `mood.md`) sind dynamische Notizen.
- Wenn ein Trait zu lang wird, schreibe ihn beim nächsten Update kompakter um.
