# **Fejlesztői Környezet Összeállítása Windows 10 és 11-en**

Ez a dokumentáció segít a fejlesztői környezet beállításában **Windows 10 és 11** alatt. Az útmutató végigvezeti a szükséges eszközök telepítésén, konfigurálásán és használatán, hogy minden fejlesztő hatékonyan tudjon dolgozni.


# Tartalomjegyzék

1. **Bevezetés**
2. **Szükséges szoftverek és eszközök**
   - 2.1. Windows 10/11 előkészítése  
   - 2.2. WSL2 és Ubuntu telepítése  
   - 2.3. Docker és Docker Compose telepítése  
   - 2.4. SQL Server 2019 konténer előkészítése  
3. **Liquibase beállítása és használata**
   - 3.1. Liquibase konténer indítása  
   - 3.2. Adatbázis inicializálása  
   - 3.3. Példák változtatások kezelésére (changelog)  
4. **Fejlesztői környezet konfigurálása**
   - 4.1. Spring Boot és Liquibase integráció  
   - 4.2. Tesztelési környezet beállítása  
5. **Hibaelhárítás és gyakori problémák**
6. **Összegzés**
---

## **1. Szükséges eszközök és funkcióik**

| **Eszköz**      | **Funkció** |
|----------------|------------|
| **Git** | Verziókezelő rendszer, amely lehetővé teszi a forráskód verziózását és a csapatmunkát. |
| **Java 17 (JDK)** | A Java alkalmazások futtatásához és fejlesztéséhez szükséges környezet. |
| **Maven** | Build eszköz, amely kezeli a függőségeket és automatizálja a Java alkalmazások fordítását és csomagolását. |
| **IntelliJ IDEA** | Fejlesztői környezet (IDE), amely támogatja a Java fejlesztést és egyéb eszközök integrációját. |
| **Docker** | Konténerizált alkalmazások futtatásához és kezeléséhez szükséges eszköz. |
| **Liquibase** | Adatbázis-migrációs eszköz, amely verziózott módon kezeli az adatbázis változásokat. |

---

## **2. Telepítés és beállítások**

Ebben a részben találod a leírás a környezet kialakításához, arra az esetre, ha nem olyan eszközön dolgoznál, amire előre fel van telepítve a fejlesztéshez szükséges eszközök. 

### **2.1 Git telepítése és beállítása**

#### **Telepítés**
1. Töltsd le és telepítsd a **Git for Windows**-t:
    - [Git letöltése](https://git-scm.com/downloads)
2. Telepítéskor ajánlott beállítások:
    - **Use Git from the Windows Command Prompt**
    - **Use the OpenSSH**
    - **Use the default credential helper**

#### **Telepítés ellenőrzése**
Futtasd a következő parancsot:
```sh
git --version
```
Ha a verzió megjelenik, a telepítés sikeres.

#### **Alap parancsok**
- `git clone <repo_url>` – Távoli repó klónozása.
- `git status` – Módosítások listázása.
- `git commit -m "Üzenet"` – Változások mentése.
- `git push` – Változások feltöltése a távoli repóba.

---

### **2.2 Java 17 telepítése és beállítása**

#### **Telepítés**
1. Töltsd le az OpenJDK 17-et az [Eclipse Temurin](https://adoptium.net/) oldaláról.
2. Telepítés után állítsd be a **JAVA_HOME** környezeti változót:
    - **Vezérlőpult** → **Rendszer** → **Speciális rendszerbeállítások** → **Környezeti változók**.
    - Hozz létre egy új **JAVA_HOME** változót, és add meg az OpenJDK elérési útját.

#### **Telepítés ellenőrzése**
Futtasd:
```sh
java -version
```
Ha a verzió megjelenik, a telepítés sikeres.

#### **Alap parancsok**
- `javac MyClass.java` – Java fájl fordítása.

---

### **2.3 Maven telepítése és beállítása**

#### **Telepítés**
1. Töltsd le a [Maven hivatalos oldaláról](https://maven.apache.org/download.cgi).
2. Csomagold ki, és add hozzá a **bin** mappáját a **Path** környezeti változókhoz.

#### **Telepítés ellenőrzése**
Futtasd:
```sh
mvn -version
```
Ha a verzió megjelenik, a telepítés sikeres.

#### **Alap parancsok**
- `mvn clean install` – Build + tesztek futtatása.
- `mvn package` – JAR fájl létrehozása.

---

### **2.4 IntelliJ IDEA telepítése**

#### **Telepítés**
1. Töltsd le az [IntelliJ IDEA Community vagy Ultimate verzióját](https://www.jetbrains.com/idea/download/).
2. Telepítés után állítsd be a megfelelő JDK-t a projektben:
    - **File** → **Project Structure** → **SDKs** → **Add JDK**.

---

https://podman-desktop.io/downloads/windows
### **2.5.1 Podman telepítése és beállítása**

#### **Telepítés**
1. Töltsd le a **Podman Desktopot** a [hivatalos oldalról](https://podman-desktop.io/downloads/windows).
A **Podman** egy nyílt forráskódú, daemon nélküli konténerkezelő eszköz, amely lehetővé teszi konténerek és podok futtatását, kezelését és fejlesztését. Windows 11 rendszeren a **Podman Desktop** alkalmazás telepítésével egyszerűen használhatjuk a Podmant.

**A telepítés lépései Windows 11-re:**

1. **Podman Desktop letöltése:**
   - Nyisd meg a [Podman Desktop hivatalos weboldalát](https://podman-desktop.io/docs/installation/windows-install).
   - Kattints a "Download Now" gombra a Windows telepítő letöltéséhez.

2. **Telepítő futtatása:**
   - Nyisd meg a letöltött `.exe` telepítőfájlt.
   - Kövesd a telepítő utasításait a Podman Desktop telepítéséhez.

3. **Rendszerkövetelmények ellenőrzése:**
   - Győződj meg arról, hogy a rendszered megfelel az alábbi követelményeknek:
     - Windows 10 vagy Windows 11.
     - Legalább 6 GB RAM.
     - A Windows Subsystem for Linux (WSL) telepítve van. Ha nincs, a Podman Desktop segít annak telepítésében.

4. **WSL és virtuális gép beállítása:**
   - A Podman a WSL-t használja a Linux-alapú konténerek futtatásához.
   - Ha a WSL nincs telepítve, nyiss meg egy parancssort rendszergazdaként, és futtasd a következő parancsot:
     ```
     wsl --install --no-distribution
     ```
     Ez telepíti a WSL-t anélkül, hogy egy konkrét Linux disztribúciót is telepítene.
   - Indítsd újra a számítógépet a változtatások érvénybe lépéséhez.

5. **Podman telepítése és indítása:**
   - A Podman Desktop első indításakor ellenőrzi a szükséges komponensek meglétét.
   - Ha a Podman nincs telepítve, a Podman Desktop felajánlja annak telepítését.
   - Kattints az "Install" gombra, és kövesd az utasításokat.
   - A telepítés után kattints az "Initialize and start" gombra a Podman gép inicializálásához és indításához.

**Alternatív telepítési módszerek:**

- **Chocolatey csomagkezelő használata:**
  - Nyiss meg egy parancssort rendszergazdaként.
  - Futtasd a következő parancsot a Podman Desktop telepítéséhez:
    ```
    choco install podman-desktop
    ```

- **Scoop csomagkezelő használata:**
  - Nyiss meg egy parancssort.
  - Add hozzá az "extras" tárolót:
    ```
    scoop bucket add extras
    ```
  - Telepítsd a Podman Desktopot:
    ```
    scoop install podman-desktop
    ```

- **Winget csomagkezelő használata:**
  - Nyiss meg egy parancssort.
  - Futtasd a következő parancsot a Podman Desktop telepítéséhez:
    ```
    winget install -e --id RedHat.Podman-Desktop
    ```

**Megjegyzés:**
A Podman Desktop telepítése során a rendszer automatikusan beállítja a szükséges környezeti változókat, így a `podman` parancs elérhető lesz a parancssorból vagy a PowerShellből. Ha egyéb terminálokat (pl. Git Bash) használsz, előfordulhat, hogy manuálisan kell hozzáadnod a Podman telepítési útvonalát a PATH környezeti változóhoz.

**További információk:**
- [Podman Desktop telepítése Windowsra](https://podman-desktop.io/docs/installation/windows-install)
- [Podman használata Windows rendszeren](https://www.redhat.com/en/blog/run-podman-windows)

Ezekkel a lépésekkel sikeresen telepítheted és használhatod a Podmant Windows 11 rendszeren. 

---

### **2.5.2 Docker telepítése és beállítása**

#### **Telepítés**
1. Töltsd le a **Docker Desktopot** a [hivatalos oldalról](https://www.docker.com/products/docker-desktop/).
2. Engedélyezd a **WSL 2 Backendet** a telepítés során.

#### **Telepítés ellenőrzése**
Futtasd:
```sh
docker --version
docker run hello-world
```
Ha a konténer fut, a telepítés sikeres.

#### **Alap parancsok**
- `docker ps` – Futtatott konténerek listázása.
- `docker images` – Letöltött képfájlok listázása.
- `docker-compose up -d` – Több konténer indítása háttérben.

---

#### **Alap parancsok**
- `docker ps` – Futtatott konténerek listázása.
- `docker images` – Letöltött képfájlok listázása.
- `docker-compose up -d` – Több konténer indítása háttérben.

### **2.6 Liquibase telepítése és beállítása**

#### **Telepítés**
1. Töltsd le a [Liquibase hivatalos oldaláról](https://www.liquibase.org/download).
2. Add hozzá a **bin** mappát a **Path** környezeti változókhoz.

#### **Telepítés ellenőrzése**
Futtasd:
```sh
liquibase --version
```
Ha a verzió megjelenik, a telepítés sikeres.

#### **Alap parancsok**
- `liquibase update` – Adatbázis frissítése a változásokkal.
- `liquibase rollback COUNT` – Adott számú változás visszavonása.

---

## **3. GitHub és Maven Token Generálás és Beállítás**

### **3.1 GitHub Personal Access Token (PAT) generálása**

1. Nyisd meg a [GitHub token generálási oldalt](https://github.com/settings/tokens).
2. Kattints a **Generate new token (classic)** gombra.
3. Adj meg egy nevet, és válaszd ki a következő jogosultságokat:
    - **repo** (olvasás/írás GitHub repókhoz)
    - **read:packages**, **write:packages** (Maven csomagtárolókhoz)
4. Másold ki a tokent, mert csak egyszer látod!

---

### **3.2 Maven, git token beállítása**

1. Nyisd meg a következő fájlt:
    - Windows: `%USERPROFILE%\.m2\settings.xml`
2. Másold be ezt a konfigurációt:

```xml
<settings>
  <servers>
    <server>
      <id>REPO_ID</id>
      <username>GITHUB_USERNAME</username>
      <password>TOKEN</password>
    </server>
  </servers>
</settings>
```

Ezzel a Maven hitelesítve lesz a GitHub csomagtárhoz.

---

## **4. Összegzés**
✔ Minden eszközt telepítettél és teszteltél.  
✔ GitHub tokened megfelelően van beállítva.  
✔ Fejlesztési környezeted készen áll!




# **Liquibase bevezető**

A **Liquibase** egy adatbázis-verziókezelő eszköz, amely lehetővé teszi különböző formátumokban (XML, YAML, JSON, SQL) megírt változtatások (changeset-ek) kezelését. Ha kombinálni szeretnéd az **SQL**, **XML** és **JSON** fájlokat, valamint biztosítani, hogy az adatbázis létrehozása **SQL DDL**-ből történjen, az alábbi megoldások segíthetnek.

---


### **1. Több formátum kombinálása (SQL + XML + JSON)**
Liquibase lehetővé teszi, hogy több formátumban írt változtatásokat egy `master` changelog fájlból hívd meg. Példa egy **XML alapú changelogra**, amely SQL és JSON változtatásokat is betölt:

#### **changelog.xml**
```xml
<databaseChangeLog>
    <!-- SQL fájl beillesztése -->
    <include file="changes/01-init-schema.sql" relativeToChangelogFile="true"/>
    
    <!-- XML changeset -->
    <changeSet id="002" author="admin">
        <createTable tableName="users">
            <column name="id" type="int" autoIncrement="true">
                <constraints primaryKey="true"/>
            </column>
            <column name="username" type="varchar(255)"/>
        </createTable>
    </changeSet>

    <!-- JSON changeset beillesztése -->
    <include file="changes/03-insert-data.json" relativeToChangelogFile="true"/>
</databaseChangeLog>
```
Ebben az esetben:
- **SQL** fájl (`01-init-schema.sql`) az adatbázis szerkezetét határozza meg.
- **XML** egy új táblát hoz létre.
- **JSON** egyes adatok feltöltéséhez használható.

#### **03-insert-data.json**
```json
{
  "databaseChangeLog": [
    {
      "changeSet": {
        "id": "003",
        "author": "admin",
        "changes": [
          {
            "insert": {
              "tableName": "users",
              "columns": [
                { "column": { "name": "id", "value": "1" } },
                { "column": { "name": "username", "value": "test_user" } }
              ]
            }
          }
        ]
      }
    }
  ]
}
```

---

### **2. Adatbázis létrehozása SQL DDL-ből**
Ha a teljes adatbázisstruktúrát **DDL SQL** fájlból szeretnéd létrehozni, akkor az SQL fájlokat az `include` vagy `loadUpdateData` parancsokkal hívhatod be.

#### **Megoldás SQL DDL használatára**
**databasechangelog.xml**
```xml
<databaseChangeLog>
    <include file="ddl/create_database.sql" relativeToChangelogFile="true"/>
</databaseChangeLog>
```
Ebben a `create_database.sql` fájl tartalmazza az összes DDL parancsot (táblák, indexek, stb. létrehozása):

```sql
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(255) NOT NULL
);
```
Ezzel az adatbázis-struktúra az SQL DDL alapján jön létre.

---

### **3. Baseline alapfogalma**
A **baseline** egy olyan mechanizmus, amely lehetővé teszi, hogy egy már létező adatbázist **Liquibase kezelhetővé** tegyél anélkül, hogy újra lefuttatná az összes régi migrációt.

#### **Baseline megvalósítása**
1. Ha egy adatbázis már létezik, és azt szeretnéd, hogy Liquibase figyelje a további változásokat, **de ne futtassa le az eddigi migrációkat**, akkor a következő lépéseket teheted:
   - **Liquibase snapshot készítése** a meglévő adatbázisról:
     ```sh
     liquibase --changeLogFile=baseline.xml generateChangeLog
     ```
   - A generált `baseline.xml` tartalmazza a meglévő szerkezetet.
   - **Baseline alkalmazása**, hogy Liquibase tudja, hogy ezt a változtatást már lefuttattad:
     ```sh
     liquibase --changeLogFile=baseline.xml tag v1.0
     ```
   - Innentől kezdve minden új változás (pl. `update`) a `v1.0` verzió utáni módosításokat veszi figyelembe.

---

### **4. Futási környezettől függő changeset-ek**
Liquibase támogatja a **context** és **labels** attribútumokat, amelyekkel megadhatod, hogy egy adott környezetben milyen változtatások fussanak le.

Példa **context** használatára:
```xml
<changeSet id="004" author="admin" context="dev">
    <insert tableName="users">
        <column name="id" value="2"/>
        <column name="username" value="dev_user"/>
    </insert>
</changeSet>

<changeSet id="005" author="admin" context="prod">
    <insert tableName="users">
        <column name="id" value="3"/>
        <column name="username" value="prod_user"/>
    </insert>
</changeSet>
```
- Ha `liquibase update --contexts=dev` parancsot futtatod, csak a **dev** környezethez tartozó changeset fut le.
- Ha `liquibase update --contexts=prod`, akkor a **prod** változások kerülnek végrehajtásra.

**Alternatív megoldás:** **labels**
```xml
<changeSet id="006" author="admin" labels="staging">
    <addColumn tableName="orders">
        <column name="staging_column" type="varchar(255)"/>
    </addColumn>
</changeSet>
```
Futtatás staging környezetben:
```sh
liquibase update --labels=staging
```

---

### **Összegzés**
✅ **SQL, XML, JSON kombinálható** egy `master` changelog segítségével.  
✅ **Adatbázis-struktúrát SQL DDL-lel is definiálhatod**, és Liquibase-el kezelheted.  
✅ **Baseline biztosítja, hogy meglévő adatbázisra tudj migrációt készíteni**, anélkül, hogy újra lefuttatnád az eddigieket.  
✅ **Környezetspecifikus changeset-ek** `context` vagy `labels` attribútummal valósíthatók meg.

Ha további részletek kellenek, szólj! 🚀







A hibaüzeneted alapján a Liquibase **checksum validation** hibát észlelt egy SQL fájl tartalmának megváltozása miatt. Ez azt jelenti, hogy **a fájl módosult**, de Liquibase még a régi ellenőrző összeget várja.

---

## **Hogyan javíthatod ezt?**

### **1. Frissítsd a Liquibase checksumot**
Ha biztos vagy benne, hogy a módosítások helyesek, akkor futtasd az alábbi parancsot a checksum frissítésére:

```sh
mvn liquibase:clearCheckSums
```
vagy, ha Gradle-t használsz:

```sh
./gradlew liquibaseClearChecksums
```

Ez törli az ellenőrző összegeket, és Liquibase újraszámolja azokat a következő futásnál.

---

### **2. Használj `runOnChange="true"` beállítást**
Ha azt szeretnéd, hogy Liquibase **automatikusan felismerje és elfogadja a változásokat**, akkor az `include`-nál ezt írd:

```xml
<include file="db/changelog/ddl/create_database.sql" relativeToChangelogFile="true" runOnChange="true"/>
```
Ez biztosítja, hogy a fájl minden módosítása után frissüljön az ellenőrző összeg.

---

### **3. Manuálisan töröld az ellenőrző összegeket az adatbázisból**
Ha az előző lépések nem működnek, akkor törölheted a **Liquibase nyilvántartását** az adatbázisban. Futtasd ezt az SQL parancsot:

```sql
DELETE FROM databasechangelog WHERE id = 'raw::includeAll';
```
Majd indítsd újra az alkalmazást.

---

### **4. Ellenőrizd az SQL fájl formátumát**
Ha az SQL fájlt **Windows-ról Linux-ra mozgattad**, előfordulhat, hogy a sortörések (`\r\n` vs `\n`) miatt más lesz a checksum. Próbáld meg újra elmenteni a fájlt **UTF-8 BOM nélkül**, vagy futtasd ezt egy Linux terminálon:

```sh
dos2unix db/changelog/ddl/create_database.sql
```

---

## **Összegzés**
1️⃣ Futtasd: `mvn liquibase:clearCheckSums`  
2️⃣ Adj `runOnChange="true"` beállítást az `include` elemhez  
3️⃣ Ha még mindig nem jó, töröld a `databasechangelog` táblából az adott rekordot  
4️⃣ Ellenőrizd, hogy a fájl formátuma nem változott-e

Ha továbbra is fennáll a probléma, írd meg, **milyen adatbázist és build rendszert (Maven/Gradle) használsz**! 🚀
