## Ablauf nach Schema-Änderung

keine lokale DB? dann:

| Aktion                             | Befehl                                |
| ---------------------------------- | ------------------------------------- |
| Schema geändert                    | `npx prisma generate`                 |
| Migration erzeugen (neue Tabellen) | `npx prisma migrate dev --name init`  |
| Testdaten einfügen                 | `npx prisma db seed`                  |
| Datenbank inspizieren              | `npx prisma studio`  //muss man nicht |

# Prisma CLI – Befehlsübersicht

| Zweck                  | Befehl                                             | Beschreibung                                                |
| ---------------------- | -------------------------------------------------- | ----------------------------------------------------------- |
| ** Installation**      | `npm install prisma --save-dev`                    | Prisma CLI lokal installieren                               |
|                        | `npm install @prisma/client`                       | Prisma Client installieren (nach `generate` nötig)          |
| ** Initialisierung**   | `npx prisma init`                                  | Erstellt `prisma/schema.prisma` + `.env`                    |
| ** Migrationen**       | `npx prisma migrate dev --name init`               | Erstellt Migration und aktualisiert DB (für Entwicklung)    |
|                        | `npx prisma migrate deploy`                        | Wendet Migrationen in Produktion an                         |
|                        | `npx prisma migrate reset`                         | Setzt Datenbank zurück und spielt Seeds erneut ein          |
| ** Schema & Client**   | `npx prisma generate`                              | Generiert Prisma Client aus `schema.prisma`                 |
|                        | `npx prisma format`                                | Formatiert Prisma-Schema automatisch                        |
| ** Seed-Daten**        | `npx prisma db seed`                               | Führt Seed-Skript (`prisma/seed.ts`) aus                    |
| ** Daten ansehen**     | `npx prisma studio`                                | Öffnet Prisma Studio (GUI zum Bearbeiten der DB)            |
| ** Datenbank prüfen**  | `npx prisma db pull`                               | Liest bestehende DB-Struktur ein → Schema wird generiert    |
|                        | `npx prisma db push`                               | Schreibt dein Schema in eine bestehende DB (ohne Migration) |
| **Debugging & Info**   | `npx prisma validate`                              | Prüft dein Schema auf Fehler                                |
|                        | `npx prisma version`                               | Zeigt aktuelle Prisma-Version                               |
|                        | `npx prisma -h`                                    | Zeigt Hilfe & alle CLI-Befehle                              |
| **Entwicklungshelfer** | `npx prisma generate --watch`                      | Regeneriert Prisma Client bei jeder Schema-Änderung         |
|                        | `npx prisma format --schema=path/to/schema.prisma` | Formatiert Schema an beliebigem Ort                         |

---
# Prisma Query Sheet

### Basis
```ts
import { PrismaClient } from '@prisma/client';
const prisma = new PrismaClient();
```
### 1. alle Datensätze holen
#### Alle Studenten
```ts
const students = await prisma.student.findMany();
```
#### Alle Kurse mit Gruppen
```ts
const courses = await prisma.course.findMany({
  include: { groups: true }
});
```
#### Alle Gruppen eines bestimmten Kurses
```ts
const groups = await prisma.group.findMany({
  where: { courseID: 1 },
  include: { course: true }
});
```
### 2. Relationen mit `include`
#### "Wer hat welche Gruppe gewählt?"
(Tabelle: Group, Student, PreferenceSubmission)
```ts
const selections = await prisma.preferenceSubmission.findMany({
  include: {
    student: true,
    group: true,
    course: true,
  }
});
```
Ergebnis:
```json
[
  {
    "id": 1,
    "student": { "firstName": "Anna", "lastName": "Müller" },
    "group": { "name": "Team A" },
    "course": { "name": "Software Engineering" }
  }
]
```
### 3. Beziehungen verschachteln (z.B. Präferenzen pro Student)
```ts
const submissions = await prisma.preferenceSubmission.findMany({
  include: {
    student: true,
    preferences: true
  }
});
```

### 4. CSV-Export (Beispiel)
```ts
import { PrismaClient } from '@prisma/client';
const prisma = new PrismaClient();

export const GET = async () => {
  const submissions = await prisma.preferenceSubmission.findMany({
    include: {
      student: true,
      group: true,
      course: true,
    }
  });

  const csv = [
    ['Student', 'Kurs', 'Gruppe'],
    ...submissions.map(s => [
      `${s.student.firstName} ${s.student.lastName}`,
      s.course.name,
      s.group.name
    ])
  ]
    .map(r => r.join(','))
    .join('\n');

  return new Response(csv, {
    headers: { 'Content-Type': 'text/csv' }
  });
};
```
URL im Browser öffnen → CSV-Download startet automatisch.
