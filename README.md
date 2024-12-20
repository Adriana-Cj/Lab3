# Lucrarea de laborator nr. 3. Bazele lucrului cu baze de date în Laravel

## Scopul lucrării
Familiarizarea cu principiile de bază ale lucrului cu baze de date în Laravel. Învățarea creării de migrații, modele și seed-uri pe baza aplicației web To-Do App.

## Condiții
În cadrul acestei lucrări de laborator, vom continua dezvoltarea aplicației To-Do App pentru echipe, începută în lucrările de laborator anterioare.

Vom adăuga funcționalitatea de lucru cu baza de date, vom crea modele și migrații, vom configura relațiile dintre modele și vom învăța să utilizam fabrici și seed-uri pentru generarea datelor de testare.

## №1. Pregătirea pentru lucru
1. Instalam SGBD-ul MySQL, PostgreSQL sau SQLite pe calculatorul dumneavoastră.
2. Cream o nouă bază de date pentru aplicația todo_app.
3. Configuram variabilele de mediu în fișierul `.env` pentru a ne conecta la baza de date:
```
DB_CONNECTION=baza_dvs_de_date (mysql, pgsql, sqlite)
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=todo_app
DB_USERNAME=utilizator_dvs
DB_PASSWORD=parola_dvs
```
![image](https://github.com/user-attachments/assets/8c782af5-a097-4ea4-b05c-8355df0bbd92)

## №2. Crearea modelelor și migrațiilor
1. Cream modelul `Category` — categoria unei sarcini.
`php artisan make:model Category -m`

![image](https://github.com/user-attachments/assets/d769e3eb-142f-4a87-b711-25b1b2068a68)

2. Definirea structurii tabelei category în migrație:
   
Adăugam câmpurile:

 - `id` — cheia primară;

 - `name` — numele categoriei;

 - `description` — descrierea categoriei;

 - `created_at` — data creării categoriei;

 - `updated_at` — data actualizării categoriei.

![image](https://github.com/user-attachments/assets/cdcebbe2-a98e-4548-83e9-d65cdbd59628)


3. Cream modelul `Task` — sarcina.

![image](https://github.com/user-attachments/assets/b47ec1d1-4322-440d-b2cd-e82df81cd2d2)

4. Definirea structurii tabelei **task** în migrație:
   
Adăugam câmpurile:

 - `id` — cheia primară;

 - `title` — titlul sarcinii;

 - `description` — descrierea sarcinii;

 - `created_at` — data creării sarcinii;

 - `updated_at` — data actualizării sarcinii.

![image](https://github.com/user-attachments/assets/809d01cf-fb77-416a-8b66-732b25b67a57)


5. Cream modelul `Tag` — eticheta unei sarcini.

![image](https://github.com/user-attachments/assets/54cb87f6-5ae7-40bd-abf3-94d5b07cfb76)
    
6. Definirea structurii tabelei tag în migrație:
    
Adăugam câmpurile:

 - `id` — cheia primară;

 - `name` — numele etichetei;

 - `created_at` — data creării etichetei;

 - `updated_at` — data actualizării etichetei.

![image](https://github.com/user-attachments/assets/98e6764e-9725-4bef-b6c6-e5306a0ed193)

7. Rulam migrarea pentru a crea tabelele în baza de date:
   
`php artisan migrate`

![image](https://github.com/user-attachments/assets/75a72cb9-cb35-41c4-a7ae-79652548fa80)

8. Adăugam câmpul $fillable în modelele `Task`, `Category` și `Tag` pentru a permite atribuirea în masă a datelor.
![image](https://github.com/user-attachments/assets/e81a6186-ef68-4aa7-b2e4-ae0c994501b1)

![image](https://github.com/user-attachments/assets/a476f3f7-a1db-4e41-b899-c4ebda51606f)

![image](https://github.com/user-attachments/assets/bdfb2dab-9a29-4981-8e09-da0778a9ac41)



## №3. Relația dintre tabele
1. Cream o migrare pentru a adăuga câmpul `category_id` în tabela task.
   
`php artisan make:migration add_category_id_to_tasks_table --table=tasks`

![image](https://github.com/user-attachments/assets/4e23d822-0e27-4e48-a89e-8361778aae92)

Definim structura câmpului `category_id` și adăugam cheia externă pentru a face legătura cu tabela **category**.

![image](https://github.com/user-attachments/assets/1ab673d7-e2f4-4d79-8df7-eb181c6600bf)
   
2. Cream o tabelă intermediară pentru relația de tipul multe-la-multe dintre sarcini și etichete:
`php artisan make:migration create_task_tag_table`

![image](https://github.com/user-attachments/assets/937370aa-734a-473c-8e79-582588a6a8ca)

3. Definirea structurii corespunzătoare a tabelei în migrație.
 - Această tabelă trebuie să lege sarcinile și etichetele prin identificatorii lor.
 - Exemplu: `task_id` și `tag_id`: sarcina `10` este legată de eticheta `5`.

![image](https://github.com/user-attachments/assets/7ba408b7-b13a-4a9b-b619-295c27f21b30)
   
4. Rulam migrarea pentru a crea tabela în baza de date.

![image](https://github.com/user-attachments/assets/45ad2dbc-332a-42d9-b48a-bd94a5a8b39b)


## №4. Relațiile dintre modele
1. Adăugam relații în modelul `Category` (O categorie poate avea multe sarcini)
Deschidem modelul `Category` și adăugam metoda:
```
public function tasks()
{
    return $this->hasMany(Task::class);
}
```
![image](https://github.com/user-attachments/assets/feda669e-e6a2-4900-abba-2c1c28b8227e)

2. Adăugam relații în modelul `Task`
 - Sarcina este legată de o categorie.
 - Sarcina poate avea multe etichete.

![image](https://github.com/user-attachments/assets/26982bcf-4419-464a-8fd2-ec89a959fafc)

3. Adăugam relații în modelul `Tag` (O etichetă poate fi legată de multe sarcini).

![image](https://github.com/user-attachments/assets/4727163b-7be5-478f-be68-f0da187f78d0)

4. Adăugam câmpurile corespunzătoare în `$fillable` ale modelelor.


## №5. Crearea fabricilor și seed-urilor
1. Cream o fabrică pentru modelul `Category`:
   
`php artisan make:factory CategoryFactory --model=Category`

![image](https://github.com/user-attachments/assets/736b9938-6c6d-4f98-8a88-731f39953476)

 - Definim structura datelor pentru generarea categoriilor.

2. Cream o fabrică pentru modelul `Task`.

![image](https://github.com/user-attachments/assets/0e9e3785-b1b0-4cae-913b-ad33c7c7e52d)

3. Cream o fabrică pentru modelul `Tag`.
   
![image](https://github.com/user-attachments/assets/33039bdf-a91a-4388-84d7-80b2fc872188)
   
4. Cream seed-uri pentru a popula tabelele cu date inițiale pentru modelele `Category`, `Task`, `Tag`.
   
**CategoryFactory.php**

![image](https://github.com/user-attachments/assets/0b9b0a7b-bf3d-4c40-8a89-e7501cfae4be)

**TaskFactory.php**

![image](https://github.com/user-attachments/assets/d3e62e34-c8b6-4c37-a1e1-3d43710ac19a)

**TagFactory.php**

![image](https://github.com/user-attachments/assets/67a29ff8-17fa-45b9-b7e3-1cdb144335c4)

5. Actualizam fișierul DatabaseSeeder pentru a lansa seed-urile și le rulam:

**DatabaseSeeder.php**
![image](https://github.com/user-attachments/assets/a4dc264f-2ca9-444c-8eb8-bcafe986ec35)

`php artisan db:seed`

![image](https://github.com/user-attachments/assets/7a6e400c-e54e-41aa-829c-c61a3413b2b3)


## №6. Lucrul cu controlere și vizualizări
1. Deschidem controlerul TaskController (app/Http/Controllers/TaskController.php).
   
3. Actualizam metoda index pentru a obține lista sarcinilor din baza de date.

![image](https://github.com/user-attachments/assets/8d9539e7-18a8-4b7d-9907-6058bfcaef87)
   
4. Folosim modelul Task pentru a obține toate sarcinile.
 - Actualizam metoda show pentru a afișa o sarcină individuală.
![image](https://github.com/user-attachments/assets/75b65eaa-8d44-4fda-9464-198aa47a4983)

 - Afișam informațiile despre sarcină după identificatorul acesteia.

 - Obligatoriu afișam categoria și etichetele sarcinii.

![image](https://github.com/user-attachments/assets/49bb6d8b-352c-43ba-8340-3d2bf25a47d5)
![image](https://github.com/user-attachments/assets/bfcd41f7-62b5-4968-aaf1-7f94f0758085)


5. În metodele index și show, folosim metoda with (Eager Loading) pentru a încărca modelele asociate.
   
6. Actualizam vizualizările corespunzătoare pentru a afișa lista de sarcini și o sarcină individuală.

![image](https://github.com/user-attachments/assets/85a45914-a63a-4c8f-ac23-a1b9ae349d39)

![image](https://github.com/user-attachments/assets/ca8fd7ed-3988-46b7-9819-61b99d2199b1)

    
7. Actualizam metoda create pentru a afișa formularul de creare a unei sarcini și metoda store pentru a salva o sarcină nouă în baza de date.
    
Notă: Deoarece nu am studiat încă formularele, folosim obiectul Request pentru a obține datele. De exemplu:
```
$request->input('title');
// sau
$request->all();
```

![image](https://github.com/user-attachments/assets/5ec9361d-1694-42df-b493-5501605845ef)


7. Actualizam metoda edit pentru a afișa formularul de editare a unei sarcini și metoda update pentru a salva modificările în baza de date.

![image](https://github.com/user-attachments/assets/6a8ed00b-6616-4e0d-92a2-fd8a8d771e3e)


9. Actualizam metoda destroy pentru a șterge o sarcină din baza de date.

![image](https://github.com/user-attachments/assets/3756e36f-63a9-4de3-9ac7-36feffafe860)


## Sarcini suplimentare
1. Cream modelul `Comment` pentru comentariile sarcinilor.

![image](https://github.com/user-attachments/assets/42764be1-0df2-48f0-88b9-a0c2c5680511)

Adăugam câmpurile corespunzătoare în migrare.

![image](https://github.com/user-attachments/assets/b7b5efa7-7150-4369-b59e-065931739c7c)

Cream relații între modelele `Task` și `Comment`.

![image](https://github.com/user-attachments/assets/76c443c4-8e2d-4b60-9030-5b4fb784fcac)
![image](https://github.com/user-attachments/assets/91903b1d-bf66-40ca-8dc0-3f3a37b5c989)

2. Adăugam posibilitatea de a adăuga comentarii la sarcini.
Actualizați vizualizarea pentru a afișa comentariile unei sarcini și pentru a vizualiza lista de comentarii și comentariul după `id`:

`/task/{id}/comment, /task/{id}/comment/{comment_id}`.

![image](https://github.com/user-attachments/assets/e75c4e14-6cc0-4cca-8cc1-e5aa6ffa272a)

![image](https://github.com/user-attachments/assets/fd696d66-f870-4825-8372-a2a4143d4b44)


3. Adăugam posibilitatea de a adăuga etichete la sarcini și folosiți tranzacții pentru a salva relațiile dintre sarcini și etichete.
```
DB::transaction(function () use ($request) {
     // Crearea sarcinii
     // Legarea etichetelor la sarcină
 });
```
![image](https://github.com/user-attachments/assets/ff2950f2-3fbe-4410-b2f3-86f839092b4e)


## Întrebări de control
**1. Ce sunt migrațiile și la ce se folosesc?**

Migrațiile sunt fișiere care permit modificarea structurii unei baze de date (crearea, modificarea sau ștergerea tabelelor) într-un mod organizat. Ele sunt folosite pentru a urmări schimbările bazei de date în timp și pentru a sincroniza structura bazei între diferite medii de dezvoltare sau producție.

**2. Ce sunt fabricile și seed-urile și cum simplifică procesul de dezvoltare și testare?**

**Fabricile** sunt clase sau funcții care generează date false pentru testare. Ele simplifică procesul de creare a datelor pentru baze de date, reducând codul repetitiv.

**Seed-urile** sunt scripturi care populează baza de date cu date inițiale sau de test. Ele sunt utile pentru a configura rapid baza cu informații necesare pentru dezvoltare sau testare.

**3. Ce este ORM? Care sunt diferențele dintre pattern-urile DataMapper și ActiveRecord?**

ORM (Object-Relational Mapping) este o tehnologie care mapează obiectele din cod la tabelele din baza de date, astfel încât să lucrezi cu baze de date folosind obiecte, nu interogări SQL.

**DataMapper** separă complet obiectele de baza de date, iar salvarea sau încărcarea datelor se face printr-un obiect intermediar.

**ActiveRecord** combină logica de bază de date cu obiectele; fiecare obiect este direct legat de un rând din tabel, iar metodele pentru salvare/încărcare sunt incluse în obiect.

**4. Care sunt avantajele utilizării unui ORM comparativ cu interogările SQL directe?**
 - Reduce cantitatea de cod SQL scris manual.
 - Este mai ușor de utilizat și de învățat pentru dezvoltatori.
 - Oferă portabilitate între diferite baze de date.
 - Include validări, relații și alte funcționalități utile pentru dezvoltare.
 - Simplifică gestionarea erorilor și a tranzacțiilor.

**5. Ce sunt tranzacțiile și de ce sunt importante în lucrul cu bazele de date?**

Tranzacțiile reprezintă un set de operații care sunt executate ca o unitate. Fie toate operațiile reușesc, fie niciuna nu se aplică. Ele sunt importante pentru a asigura integritatea datelor, mai ales în situații critice, precum transferurile bancare sau actualizările simultane în bază.
