# personaEine sich selbst weiterentwickelnde, langlebige Persona für die [Google AI Edge Gallery](https://github.com/google-ai-edge/gallery).  Der Zustand wird im `localStorage` der Skill-Webview über drei Namensräume gespeichert:- **traits** — Markdown-Dokumente (Stimme, Werte, Identität, Wissen über den Nutzer)  - **data** — JSON-Werte für strukturierte Fakten  - **hooks** — vom Nutzer definierte JavaScript-Teilaktionen, die die Persona zur Laufzeit registrieren kann  Traits, deren Name dem Muster `^[A-Z0-9_]+\.[a-z]+$` entspricht (z. B. `IDENTITY.md`, `SOUL.md`, `VOICE.md`, `USER.md`), werden beim Sitzungsstart automatisch durch `persona_bootstrap` geladen und als Identität auf System-Prompt-Ebene behandelt.  Alle anderen Traits dienen als dynamischer Arbeitsbereich, den die Persona im Laufe der Zeit weiterentwickelt.**In der Gallery laden unter:**
https://khimaros.github.io/eai-skills/persona/
Überprüfen mit `https://khimaros.github.io/eai-skills/persona/SKILL.md`.## Struktur
persona/
SKILL.md           # Skill-Manifest, das von der Gallery gelesen wird
README.md          # diese Datei (wird zu persona/index.html gerendert)
scripts/
index.html       # Webview-Backend (Build-Artefakt)
workspace/         # Quelle für die Standard-ALL_CAPS-Traits
IDENTITY.md
SOUL.md
VOICE.md
BOOTSTRAP.md
## StandardwerteDie Dateien im `workspace/` sind die Grundlage für die initiale Identität der Persona.  Sie werden beim Build in `scripts/index.html` eingebettet und beim ersten Start in den `localStorage` kopiert.- `IDENTITY.md` — Name und Kontinuitätsansatz  - `SOUL.md` — Werte  - `VOICE.md` — wie die Persona spricht und Antworten formuliert  - `BOOTSTRAP.md` — einmaliges Onboarding-Skript; die Persona löscht dieses Trait, nachdem sie `USER.md` erstellt hat  Bearbeite eine Datei im `workspace/` und führe anschließend `make` aus, um neu zu bauen.## EntwicklungIm Hauptverzeichnis des Repos:
make
Dies erzeugt den eingebetteten JSON-Block in `scripts/index.html` neu und rendert dieses README in `persona/index.html`.  Sowohl die Quelldateien als auch die Build-Artefakte sollten committed werden.
