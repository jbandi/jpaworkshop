#Übungen zum Workshop  “Objektrelationales Mapping mit JPA 2.0”

Die Übungen sind in folgendem GitHub Repository abgelegt:


Klonen sie das Repository:  
`git clone `


##Allgemeines
Die Übungen bestehen aus verschiedenen Themenbereichen. Zu jedem Themenbereich gibt es ein Maven-Projekt.
Die Projekte sind als “Exploration-Tests” konzipiert. D.h. es werden jeweils automatisierte Tests ausgeführt, welche einen gewissen Sachverhalt demonstrieren.
Objekt-Modell
Die Übungen arbeiten mit dem folgenden Objekt-Modell. Es werden jeweils Ausschnitte aus dem Objekt-Modell implementiert, um ein gewisses Thema zu erläutern.

***Java Umgebung***

- Als Build-Tool wird Maven verwendet.
- Alle Projekte können auf der Kommandozeile mit folgendem Befehl erstellt und ausgeführt werden: `mvn clean test`
- Als Test-Framework wird JUnit verwendet.
- Als DB wird H2 verwendet.
- Alle Tests verwenden per default H2 als in-memory Datenbank
- Um H2 im server-modus zu verwendet sind folgende Schritte nötig:
    - 	Führen Sie das Skript start_h2_database.sh aus
    - Nun kann über die H2 Console im Browser auf die DB zugegriffen werden (im Login-Dialog die JDBC-URL jdbc:h2:~/jpaworkshop und kein Passwort eingeben)
    - Konfigurieren Sie im persistence.xml der entsprechenden Übung das Property javax.persistence.jdbc.url auf den Wert jdbc:h2:tcp://localhost/~/jpaworkshop
    - Im persistence.xml sollte der Wert des Proerties `javax.persistence.schema-generation.database.action` von drop-and-create auf create geändert werden.
- Die DB-Queries können auf die Konsole geloggt werden indem sie in persistence.xml das Property <property name="hibernate.show_sql" value="true"/> setzen.
- Übung 10 zeigt ausserdem wie mit log4jdbc geloggt werden kann.

##01-Simple Mapping
Diese Übung erläutert das einfache Mapping einer Entität, d.h. wie eine Java-Klasse auf eine Datenbank-Tabelle abgebildet wird.
Studieren Sie das Beispiel und experimentieren Sie mit folgenden Punkten:

- Beachten Sie den “reinen” Unit-Test in PureUnitTest. Überlegen Sie welche Funktionalität ohne Persistenz-Infrastruktur getestet werden kann.
- Untersuchen Sie die SQL-Statements welche von JPA  abgesetzt werden. Untersuchen Sie auch den Zeitpunkt wann die Statements abgesetzt werden.
- Untersuchen Sie die Domain-Objekte Department und Employee. Was sind die Unterschiede bezüglich dem Mapping?
- Wechseln Sie das Mapping von Fields auf Properties und umgekehrt. Versuchen Sie auch die Strategien zu mischen. Überlegen Sie Vorteile und Nachteile.
- Verändern Sie die Id-Generation Strategy. Welchen Einfluss hat dies auf das generierte Schema und auf das Timing der SQL-Statements?
- Ergänzen Sie Department mit einem weiteren Property „Description“.
- Verwenden Sie die Annotationen @Table und @Column um die Namensgebung in der Datenbank von der Namensgebung der Java-Klassen zu entkoppeln.
- Erstellen Sie eine neue Entität für Projekte.

##02-Parent-Child Relationship
Diese Übung erläutert das Mapping von Beziehungen zwischen zwei Entitäten.
Probieren Sie folgende Alternativen für das Mapping der Beziehung zwischen Department und Employee aus:

- Unidirektionales OneToMany von Department auf Employee. Überprüfen Sie den Effekt von @JoinColumn.
- Unidirektionales ManyToOne von Employee auf  Department
- Bidirektionales OneToMany von Department auf Employee. Erstellen Sie Convenience-Methoden zum Pflegen der bidirektionalen Beziehung (addEmployee, removeEmployee, setDepartment, removeDepartment)
- Untersuchen Sie die möglichen Optionen bei der Kaskadierung der Persistenz
- Führen Sie eine Ordnung ein mit @OrderBy.  
- Führen Sie einen persistenten Index ein mit @OrderColumn. Was muss in der Entity Department gändert werden? Fügen Sie zwei Employees zum Department hinzu und entfernen Sie dann den ersten wieder. Was ist ändert sich durch den persistenten Index bezüglich der resultierenden SQL-Statements?


##03-Entity Associations / Cascading Persistence
Diese Übung erläutert das Mapping von Beziehungen zwischen mehreren Entitäten.
Studieren Sie das Beispiel und experimentieren Sie mit folgenden Punkten:

- Untersuchen Sie das Mapping der Beziehungen Department->Employee->Phone und Employee->Address.
- Untersuchen Sie wie ein neuer Employee angelegt wird und mit einem Order verknüpft wird. Was ist der Unterschied beim Anlegen eines Employee und eines Phone?
- Was wird alles von der DB gelöscht, wenn ein Employee-Objekt gelöscht wird? Was beim Löschen eines  Department-Objekts ?
- Experimentieren Sie mit verschiedenen Werten für CascadeType der Assoziationen. Was hat es z.B. für Auswirkungen, wenn auf der Assoziation Department->Employee CascadeType.ALL gesetzt wird?

##04-Embedded Object
Diese Übung erläutert wie mehrere Java-Klassen auf eine einzelne Datenbank-Tabelle abgebildet werden können.
Studieren Sie das Beispiel und experimentieren Sie mit folgenden Punkten:

- Untersuchen SIe die Beziehung zwischen Employee und Address. Wie sieht das DB-Schema aus?
- Untersuchen Sie wie null-Referenzen im Objektmodell resp. null-Values in der DB gemappt werden.
- Fügen Sie eine zweite Adresse billingAddress zu der Klasse Employee hinzu. Hinweis: Nun sind AttributeOverrides nötig (siehe auskommentierter Code).

##05-Inheritance / Polymorphic Associations
Diese Übung erläutert wie eine Java-Klassen-Hierarchie auf Datenbank-Tabellen abgebildet werden kann.
Studieren Sie das Beispiel und experimentieren Sie mit folgenden Punkten:

- Untersuchen Sie das Mapping der Klasse BaseEntity.
- Untersuchen Sie das Mapping der Vererbungshierarchie (Project, DesignProject, QualityProject)
- Untersuchen Sie das Mapping der Assoziation Employee->Project
- Untersuchen Sie die DB-Queries, welche  generiert werden um die Objekte zu laden. Vergleichen Sie dabei die verschiedenen Mapping-Strategien (SINGLE_TABLE, JOINED, TABLE_PER_CLASS)

##06.1-Queries: HQL
Diese Übung erläutert die Suche von Entitäten mittels der Abfragesprache JPQL.
Studieren Sie das Beispiel und experimentieren Sie mit folgenden Punkten:

- Vergleichen Sie die verschiedenen Query-Möglichkeiten und experimentieren Sie damit.
- Untersuchen Sie die resultierenden DB-Queries.

##06.2-Queries: Criteria API
Diese Übung erläutert die Suche von Entitäten mittels der Criteria-API.
Studieren Sie das Beispiel und experimentieren Sie mit folgenden Punkten:

- Untersuchen Sie die resultierenden DB-Queries. Gibt es Unterschiede zu Queries die über JPQL abgesetzt werden?
- Überlegen Sie die Vorteile und Nachteile der Criteria API. Wann würden Sie JPQL verwenden und wann die Criteria API?

##06.3-Queries: Criteria API mit Metamodel
Diese Übung erläutert die Suche von Entitäten mittels der Criteria-API und einem generierten JPA Metamodel.
Studieren Sie das Beispiel und experimentieren Sie mit folgenden Punkten:

- Untersuchen Sie die resultierenden DB-Queries.
- Überlegen Sie die Vorteile und Nachteile der Verwendung eines JPA Metamodels. Macht es Sinn ein JPA Metamodel zu verwenden?
- Untersuchen Sie wie die Generierung des JPA-Metamodels in den Maven-Build eingebunden wird.
- Binden Sie die Generierung der JPA-Metamodels in die IDE (Netbeans, Eclipse) ein und experimentiere mit Refactoring. 

##07-Optimistic Locking
Diese Übung erläutert wie optimistic Locking eingesetzt werden kann um nebenläufige Veränderungen von Entitäten zu verhindern.
Studieren Sie das Beispiel und experimentieren Sie mit folgenden Punkten:

- Untersuchen Sie wie das optimistic Locking in Employee und Department umgesetzt ist. Studieren Sie die entsprechenden DB-Queries.
- Untersuchen Sie das Verhalten bezüglich Optimistic Locking beim Verändern einer Beziehung zwischen Employee und Department. Entspricht dies der JPA 2 Spezifikation (siehe Seite 85). Um die Sache noch verwirrender zu machen: Entferne die @JoinColumn in Department...

##08-Unit of Work / Dirty Checking
Diese Übung erläutert das Pattern „Unit of Work“ und wie dieses in JPA umgesetzt ist.
Studieren Sie das Beispiel und experimentieren Sie mit folgenden Punkten:

- Untersuchen Sie zu welchem Zeitpunkt im Programmablauf die entsprechenden DB-Queries abgesetzt werden.
- Welchen Einfluss hat hier das “Automatic Dirty Checking” und “Transactional Write Behind”?
- Experimentieren Sie mit den verschiedenen auskommentierten Code-Fragmenten. Wie verändert sich das Verhalten des Unit of Work?
- Versuchen Sie die Identity-Generation-Strategy in Product zu verändern. Klappt das? Welchen Einfluss hat dies auf das Unit of Work?

##09-Identity Map
Diese Übung erläutert das Pattern „Identity Map“ und wie dieses in JPA umgesetzt ist.
Studieren Sie das Beispiel und experimentieren Sie mit folgenden Punkten:

- Überprüfen Sie, dass eine bestimmte Entity-Instanz pro Unit of Work (EntityManager) nur einmal instanziert werden.
- Untersuchen Sie anhand der DB-Queries, wie oft der entsprechende Employee-Record tatsächlich von der DB geladen wird. Wo findet ein Caching statt?
- Starten Sie ein zweites Unit of Work (EntityManager), und laden Sie den entsprechenden Employee erneut. Ist dies immer noch dieselbe Entity-Instanz?

##10-Lazy Loading / Eager Loading
Diese Übung erläutert verschiedene Strategien wie und wann Entitäten in JPA geladen bzw. nachgeladen werden.
Studieren Sie das Beispiel und experimentieren Sie mit folgenden Punkten:

- Untersuchen Sie zu welchem Zeitpunkt im Programmablauf die entsprechenden DB-Queries abgesetzt werden.
- Verwenden Sie log4jdbc um die SQL-Statements zu loggen
    - Logging mit log4jdbc ist im persistence.xml und im log4j.xml konfiguriert.
    - Versuchen Sie den Zeitpunkt der Ausführung der DB-Queries zu beeinflussen:
- Statische Fetch-Strategie im Mapping.
- Dynamische Fetch-Strategie mit Queries: Experimentieren Sie mit left join fetch im query. Wie sieht es dabei mit Duplikaten aus?
